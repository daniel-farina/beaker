# Beaker

<p align="center">
<a href="https://docs.osmosis.zone/developing/dapps/get_started/">
    <img src="assets/beaker.png" alt="Beaker logo" title="Beaker" align="center" height="150" />
</a>
</p>


<p align="center" width="100%">
    <img  height="20" src="https://github.com/osmosis-labs/beaker/actions/workflows/doctest.yml/badge.svg">
    <img height="20" src="https://github.com/osmosis-labs/beaker/actions/workflows/lint.yml/badge.svg">
    <a href="https://github.com/osmosis-labs/beaker/blob/main/LICENSE-APACHE"><img height="20" src="https://img.shields.io/badge/license-APACHE-blue.svg"></a>
    <a href="https://github.com/osmosis-labs/beaker/blob/main/LICENSE-MIT"><img height="20" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
    <a href="https://deps.rs/repo/github/osmosis-labs/beaker"><img height="20" src="https://deps.rs/repo/github/osmosis-labs/beaker/status.svg"></a>
    <a href="https://crates.io/crates/beaker"><img height="20" src="https://img.shields.io/crates/v/beaker.svg"></a>
</p>

Beaker makes it easy to scaffold a new cosmwasm app, with all of the dependencies for osmosis hooked up, and a sample front-end at the ready.

## Getting Started

Install beaker with `cargo install beaker`.

Then create new project with

```sh
beaker new counter-dapp
```

After that we can create new contract (the command uses template from [cw-template](https://github.com/InterWasm/cw-template))
```sh
cd counter-dapp
beaker wasm new counter
```

### Deploy contract on LocalOsmosis

Make sure LocalOsmosis has been started (see: https://github.com/osmosis-labs/LocalOsmosis).

After that, `counter` contract can be deployed (build + store-code + instantiate) using the following command:

```sh
beaker wasm deploy counter --signer-account test1 --no-wasm-opt --raw '{ "count": 0 }'
```
The flag `--no-wasm-opt` is skipping [rust-optimizer](https://github.com/CosmWasm/rust-optimizer) for faster development iteration. For mainnet deployment, use:

```sh
beaker wasm deploy counter --signer-account <ACCOUNT> --raw '{ "count": 0 }' --network mainnet
```

Instantiate message can be stored for later use:

```sh
mkdir contracts/counter/instantiate-msgs
echo '{ "count": 0 }' > contracts/counter/instantiate-msgs/default.json
beaker wasm deploy counter --signer-account test1 --no-wasm-opt
```

### Console

After deployed, you can play with the deployed contract using:

```sh
beaker console
```

This will launch custom node repl, where `contract`, `account` are available. 
`contract` contains deployed contract.
`account` contains [pre-defined accounts in localosmosis](https://github.com/osmosis-labs/LocalOsmosis#accounts).

So you can interact with the recently deployed contract like this:

```js
await contract.counter.execute({ "increment": {}}).by(account.test1)
await contract.counter.query({ "get_count": {}})
```

You can remove `contract` and/or `account` namespace by changing config.

```toml
# Beaker.toml

[console]
account_namespace = false
contract_namespace = false
```

```js
await counter.execute({ "increment": {}}).by(test1)
await counter.query({ "get_count": {}})
```

Beaker console is also allowed to deploy contract, so that you don't another terminal tab to do so.

```js
.deploy counter -- --signer-account test1 --raw '{ "count": 999 }'
```


### Frontend

Beaker project template also come with frontend template.

```sh
cd frontend
yarn && yarn dev
```

Then open `http://localhost:3000/` in the browser.

To interact, you need to [add LocalOsmosis to keplr](https://github.com/osmosis-labs/LocalOsmosis/tree/main/localKeplr).

---

## License

The crates in this repository are licensed under either of the following licenses, at your discretion.

    Apache License Version 2.0 (LICENSE-APACHE or apache.org license link)
    MIT license (LICENSE-MIT or opensource.org license link)

Unless you explicitly state otherwise, any contribution submitted for inclusion in this library by you shall be dual licensed as above (as defined in the Apache v2 License), without any additional terms or conditions.
