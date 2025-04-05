# Rust SDK

Solana integration SDK for WOOFi Swap. ProgramId on mainnet: **WooFiGFK9x5FBYdvMg3pJBpAkPA8EEYiLcopwrxJvDG**

SDK git repo: [https://github.com/woonetwork/woofi-lz-sdk](https://github.com/woonetwork/woofi-lz-sdk)

## Rust SDK Overview:

1. sdk/src/lib.rs have functions to compose woofi swap instructions
2. client/src/main.rs have the demo usage of these instrucionts

## Rust SDK Usage:

Build:

```
cargo build
```

To test, simply run:

```
./woofi-cli try-query SOL USDC 1000000000
./woofi-cli swap USDC SOL 100000 9000
```

above try-query is query price for 1 SOL above swap will swap 0.1 USDC to SOL After swap, it will print out transaction signature and program return log

```
transaction sent, signature:63AasGTbMBQgHj9RdJ9kz5F7ZxQ9Q7nKBDoR2qHs5d6rjCC2s2tyepa6S2gDv9M5GUG4KVkNqPbDDqDsctWwA7PG
please view on explorer
Program WooFiGFK9x5FBYdvMg3pJBpAkPA8EEYiLcopwrxJvDG invoke [1]
Program log: Instruction: Swap
Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA invoke [2]
Program log: Instruction: Transfer
Program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA consumed 4645 of 168578 compute units
...
```

## Notice

Above cmd will use

```
RPC URL: https://api.mainnet-beta.solana.com 
Payer Key: ~/.config/solana/id.json
```

If you want change your own RPC URL or Payer Key, please use the following cmd:

```
./woofi-cli try-query SOL USDC 1000000000 --http-url https://api.mainnet-beta.solana.com --payer-path ~/.config/solana/id.json
./woofi-cli swap USDC SOL 100000 9000 --http-url https://api.mainnet-beta.solana.com --payer-path ~/.config/solana/id.json
```
