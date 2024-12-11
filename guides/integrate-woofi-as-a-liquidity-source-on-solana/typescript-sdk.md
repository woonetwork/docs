# TypeScript SDK

Use the SDK to interact with a deployed WOOFi sPMM program via Typescript.

### Typescript SDK Features

With the Typescript SDK, you can easily do the following:

* Use the WoofiClient or construct your own transactions with the raw instructions to:
  * Swap tokens on WOOFi sPMM contract.
* Quotes - helper functions to help developers perform quote estimations on tasks such as:
  * Get a quote on a swap and swap fee off-chain.
  * Get a quote on a swap and swap fee on-chain.
* Utility classes
  * Get contract supported tokens.
  * Get pyth oracle for supported tokens (in USD).
  * Other helper functions to help interact with Woo sPMM components

### Installation & Test

In your package, run:

```
yarn
yarn build
```

### Usage

### Run Typescript tests via local validator

In the WOOFi\_Solana/sdk folder, run:

```
yarn test
```

### Generate TypeDoc

In the `sdk` folder, run `yarn run docs`

You can also see the generated TypeDoc.

### Sample Codes

You can find sample code covering basic operations here.
