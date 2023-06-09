# \[v1] On-chain price feeds

### Intro

In order to simulate the order book from centralized exchanges without sacrificing capital efficiency, WOOFi created a custom on-chain price fee which works along side with [the sPMM algorithm](broken-reference). This price feed considers multiple parameters including mid-price $$p$$, spread $$s$$, and liquidity coefficient $$k$$.

WOOFi's on-chain price feeds are deployed on multiple blockchains. Users can call `price` to get `priceNow` and `feasible` of token. If `feasible` is `false`, do not use `priceNow` value. All prices are USDT/USDC-based, and the decimal of `priceNow` is `18 - base_decimal + quote_decimal`.

* BNB Chain Contract address: [`0x6b6fBEc7934b104e81b2046D24A990e03e17afDC`](https://bscscan.com/address/0x6b6fBEc7934b104e81b2046D24A990e03e17afDC#code)
* Avalanche C-Chain Contract address: [`0x25a4d4a094A084c7ad45Ac273cF7D6B6bfae7D4E`](https://snowtrace.io/address/0x25a4d4a094A084c7ad45Ac273cF7D6B6bfae7D4E#code)
* Fantom Contract address: [`0x209102c0D2E34282494114ea76D5251c8e7Ea7ab`](https://ftmscan.com/address/0x209102c0d2e34282494114ea76d5251c8e7ea7ab#code)
* Polygon Contract address: [`0x2Fe5E5D341cFFa606a5d9DA1B6B646a381B0f7ec`](https://polygonscan.com/address/0x2Fe5E5D341cFFa606a5d9DA1B6B646a381B0f7ec#code)

{% hint style="warning" %}
While the price feed is on-chain and permissionless, and tradeable by any user on WOOFi, we DO NOT garanutee the availability or accuracy of it. The price feed may become unavailable when the centralized liquidity source is in maintenance or any time the operator deems necessary. In no case, WOOFi is liable for any loss that might be caused by using the price feed by any third party.&#x20;
{% endhint %}

### Code Example

#### Contract ABI

```json
[
    {
        "inputs": [
            {
                "internalType": "address",
                "name": "base",
                "type": "address"
            }
        ],
        "name": "price",
        "outputs": [
            {
                "internalType": "uint256",
                "name": "priceNow",
                "type": "uint256"
            },
            {
                "internalType": "bool",
                "name": "feasible",
                "type": "bool"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]
```

#### Python Example (python 3.8)

* Query price

```python
import web3
import json

web3_provider = f'your-web3-provider'

w3 = web3.Web3(web3.Web3.HTTPProvider(web3_provider))
abi_str = '[{"inputs": [{"internalType": "address","name": "base","type": "address"}],"name": "price","outputs": [{"internalType": "uint256","name": "priceNow","type": "uint256"},{"internalType": "bool","name": "feasible","type": "bool"}],"stateMutability": "view","type": "function"}]'
abi = json.loads(abi_str)
oracle_addr = '0x6b6fBEc7934b104e81b2046D24A990e03e17afDC'
contract = w3.eth.contract(address=oracle_addr, abi=abi)

# query WOO/USDT pair quoting price 
woo_addr = '0x4691937a7508860F876c9c0a2a617E7d9E945D4B'
contract.functions.price(woo_addr).call()
```

* Response

```python
# return [priceNow, feasible]
>> [841915000000000000, True]
```
