# Earn & Stake

### General Info

* The base endpoint is: [**https://api.woofi.com**](https://api.woofi.com/). The previous host `fi-api.woo.org` still works.
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

| Name    |  Type  | Description                                                                                                                                |
| ------- | :----: | ------------------------------------------------------------------------------------------------------------------------------------------ |
| network | String | option: `bsc` / `avax` / `polygon` / `arbitrum` / `optimism` / `zksync` / `base` / `mantle` / `linea` / `sonic` / `berachain` / `hyperevm` |

**Returns**

| Name              |  Type  | Description                                                   |
| ----------------- | :----: | ------------------------------------------------------------- |
| total\_deposit    | String | total deposited(value multiplied by`10^<decimals>`)(unit:USD) |
| auto\_compounding | Object | `auto-compounded`yield aggregator                             |

**Vault list**

| Name              |  Type  | Description                       |
| ----------------- | :----: | --------------------------------- |
| `<Vault Address>` | Object | deposited token's `vault_address` |

**Vault fields**

| Name                               |   Type  | Description                                                                                         |
| ---------------------------------- | :-----: | --------------------------------------------------------------------------------------------------- |
| symbol                             |  String | target token`symbol`                                                                                |
| source                             |  String | vault type. Earn vaults return `woofi_super_charger_v2`; older vaults return `woofi_super_charger`. |
| apy                                | Decimal | APY of the vault, including compounding (unit: `%`)                                                 |
| tvl                                |  String | total deposit of single pool(value multiplied by`10^<decimals>`)(unit:USD)                          |
| price                              | Decimal | `USD price of symbol`                                                                               |
| share\_price                       |  String | vault tokens per share, multiplied by `10^18`                                                       |
| decimals                           | Integer | `symbol decimals`, for `tvl calculation`                                                            |
| loan\_assets\_percentage           | Decimal | share of the vault currently lent to WOOFi's market maker (unit: `%`)                               |
| loan\_interest\_apr                | Decimal | borrow rate paid by WOOFi's market maker on lent assets (unit: `%`)                                 |
| reserve\_vault\_assets\_percentage | Decimal | share of the vault not lent out (unit: `%`)                                                         |
| reserve\_vault\_apr                | Decimal | yield on assets not lent out (unit: `%`); `0` on vaults with no reserve strategy                    |
| weighted\_average\_apr             | Decimal | blended rate across lent and unlent assets, before compounding (unit: `%`)                          |

```json
"0x5d9DF33493747548ee033d76bCf43b9c54a91a83": {
  "symbol": "NVDAB",
  "source": "woofi_super_charger_v2",
  "apy": 0.0,
  "tvl": "2827972343598914469888",
  "price": 160.76408,
  "share_price": "1000000000000000000",
  "decimals": 18,
  "loan_assets_percentage": 0.0,
  "loan_interest_apr": 6.0,
  "reserve_vault_assets_percentage": 100.0,
  "reserve_vault_apr": 0.0,
  "weighted_average_apr": 0.0
}
```
