# Earn & Stake

### General Info

* The base endpoint is: [**https://fi-api.woo.org**](https://fi-api.woo.org/)
* All endpoints return JSON object.

### Staking

`GET /stakingv2`

**Returns**

| Name      |  Type | Description                                  |
| --------- | :---: | -------------------------------------------- |
| avg\_apr  | float | Average APR of all stakers                   |
| base\_apr | float | Base apr based on the user's staked WOO only |

### Earn

`GET /yield`

**Params**

| Name    |  Type  | Description                                                                                 |
| ------- | :----: | ------------------------------------------------------------------------------------------- |
| network | String | option: `bsc` / `avax`  / `polygon`/ `arbitrum` / `optimism` / `zksync` / `base` / `mantle` |

**Returns**

| Name              |  Type  | Description                                                   |
| ----------------- | :----: | ------------------------------------------------------------- |
| total\_deposit    | String | total deposited(value multiplied by`10^<decimals>`)(unit:USD) |
| auto\_compounding | Object | `auto-compounded`yield aggregator                             |

**Auto-Compounding Returns**

| Name              |  Type  | Description                       |
| ----------------- | :----: | --------------------------------- |
| `<Vault Address>` | Object | deposited token's `vault_address` |

**Auto-Compounding Symbol Returns**

| Name         |   Type  | Description                                                                |
| ------------ | :-----: | -------------------------------------------------------------------------- |
| symbol       |  String | target token`symbol`                                                       |
| source       |  String | target farm(lowercase)                                                     |
| apy          | Decimal | auto-compounded APR(unit: `%`)                                             |
| tvl          |  String | total deposit of single pool(value multiplied by`10^<decimals>`)(unit:USD) |
| price        | Decimal | `USD price of symbol`                                                      |
| share\_price |  String | net asset value per share                                                  |
| decimals     | Integer | `symbol decimals`, for `tvl calculation`                                   |
