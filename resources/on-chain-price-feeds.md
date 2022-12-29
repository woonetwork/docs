# \[V2] On-chain price feeds

##

### Intro

In order to simulate the order book from centralized exchanges without sacrificing capital efficiency, WOOFi created a custom on-chain price fee which works along side with [the sPMM algorithm](broken-reference). This price feed considers multiple parameters including mid-price $$p$$, spread $$s$$, and liquidity coefficient $$k$$.

The V2 of the price feeds added Chainlink price feeds as a fail safe, and it's currently only deployed on Arbitrum. Users can call `price` to get `priceNow` and `feasible` of token. If `feasible` is `false`, do not use `priceNow` value. All prices are USDT/USDC-based, and the decimal of `priceNow` is `8 (the same in decimals as price in Chainlink Oracle)`

* Arbitrum address: [0x37a9dE70b6734dFCA54395D8061d9411D9910739](https://arbiscan.io/address/0x37a9de70b6734dfca54395d8061d9411d9910739#code)

{% hint style="warning" %}
While the price feed is on-chain and permissionless, and tradeable by any user on WOOFi, we DO NOT garanutee the availability or accuracy of it. The price feed may become unavailable when the centralized liquidity source is in maintenance or any time the operator deems necessary. In no case, WOOFi is liable for any loss that might be caused by using the price feed by any third party.&#x20;
{% endhint %}

### Code Example

#### WooracleV2 Interface

```solidity
    /**
     * @notice Returns the ChainLink price of the base token / quote token
     * @param the base token address
     * @return the Chainlink price and the price's last-update timestamp
     */
    function cloPrice(address base) external view returns (uint256 price, uint256 timestamp);

    /**
     * @notice Returns the Woo self-posted price of the base token / quote token
     * @param the base token address
     * @return the Wooracle price and the price's last-update timestamp
     */
    function woPrice(address base) external view returns (uint128 price, uint256 timestamp);

    /**
     * @notice Returns the Woo self-posted price if available, otherwise fallback to ChainLink's price.
     * @dev the price is in base token / quote token, and decimal is same as the chainlink
     *   price decimal
     * @param the base token address
     * @return the compositive price and the flag indicating the price is available or not.
     */
    function price(address base) external view returns (uint256 priceNow, bool feasible);

    /**
     * Returns the detailed state of the current base token in Wooracle.
     *
     * struct State {
     *   uint128 price;
     *   uint64 spread;
     *   uint64 coeff;
     *   bool woFeasible;
     * }
     *
     * @param the base token address
     * @return the state
     */
    function state(address base) external view returns (State memory);
```

#### ABI

Abi json could be exported in chain explorer. e.g. for Arbitrum: [WooracleV2 Code](https://arbiscan.io/address/0x962d37fb9d75fe1af9aab323727183e4eae1322d#code)

#### Python Example (python 3.8)

* Query price

```python
import web3
import json

web3_provider = f'your-web3-provider'

w3 = web3.Web3(web3.Web3.HTTPProvider(web3_provider))
abi_str = '[{"inputs": [{"internalType": "address","name": "base","type": "address"}],"name": "price","outputs": [{"internalType": "uint256","name": "priceNow","type": "uint256"},{"internalType": "bool","name": "feasible","type": "bool"}],"stateMutability": "view","type": "function"}]'
abi = json.loads(abi_str)
oracle_addr = '0x962d37fb9d75fe1af9aab323727183e4eae1322d'
contract = w3.eth.contract(address=oracle_addr, abi=abi)

# query WOO/USDT pair quoting price 
woo_addr = '0xcAFcD85D8ca7Ad1e1C6F82F651fA15E33AEfD07b'
contract.functions.price(woo_addr).call()
```

* Response

```python
# return [priceNow, feasible]
>> [181915000000000000, True]
```