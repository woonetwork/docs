# Swap

### General Info

* The base endpoint is: [**https://fi-api.woo.org**](https://fi-api.woo.org/)
* All endpoints return JSON object.

### Stat

`GET /stat`

**Parameters**

| Name    | Type   |                               Description                              |
| ------- | ------ | :--------------------------------------------------------------------: |
| network | String | option `bsc` / `avax` / `fantom` / `polygon` / `arbitrum` / `optimism` |
| period  | String |    optional: `1d` / `1w` / `1m` / `3m` / `1y` / `all` default: `1m`    |

**period: 1d, Returns**

| Name        |  Type  | Description                                           |
| ----------- | :----: | ----------------------------------------------------- |
| id          | String | `timestamp / 3600`roun down                           |
| timestamp   | String | timestamp on the hour                                 |
| volume\_usd | String | hourly volume(value multiplied by 10^18)(unit: `USD`) |
| traders     | String | hourly number of address per hour                     |
| txs         | String | hourly number of trades per hour                      |

**period: 1w / 1m / 3m / 1y / all, Returns**

| Name                       |   Type  | Description                                          |
| -------------------------- | :-----: | ---------------------------------------------------- |
| id                         |  String | `timestamp / 86400`round down                        |
| timestamp                  |  String | UTC 0                                                |
| volume\_usd                |  String | Daily volume(value multiplied by 10^18)(unit: `USD`) |
| traders                    |  String | daily number of address                              |
| txs                        |  String | daily number of trades                               |
| turnover\_rate\_percentage | Decimal | daily turnover rate (unit: `%`)                      |

### Source Stat

`GET /source_stat`

**Params**

| Name    |  Type  | Description                                                      |
| ------- | :----: | ---------------------------------------------------------------- |
| network | String | default `''` option `bsc` / `avax` / `fantom` / `polygon`        |
| period  | String | optional: `1d` / `1w` / `1m` / `3m` / `1y` / `all` default: `1m` |

**Returns**

| Name        |   Type  | Description                                           |
| ----------- | :-----: | ----------------------------------------------------- |
| id          |  String | source id                                             |
| name        |  String | source name                                           |
| volume\_usd |  String | source volume(value multiplied by 10^18)(unit: `USD`) |
| txs         | Integer | source number of trades                               |
| percentage  | Decimal | pie chart(unit: `%`)                                  |
