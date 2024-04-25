# On-chain price feeds

### Intro

In order to simulate the order book from centralized exchanges without sacrificing capital efficiency, WOOFi created a custom on-chain price fee which works along side with [the sPMM algorithm](the-math-behind-spmm.md). This price feed considers multiple parameters including mid-price `p`, spread `s`, and liquidity coefficient `k`.

The price feeds use Chainlink price feeds as a fail safe. Users can call `price` to get `priceNow` and `feasible` of token. If `feasible` is `false`, do not use `priceNow` value. All prices are USDT/USDC-based, and the decimal of `priceNow` is `8 (the same in decimals as price in Chainlink Oracle)`

**Wooracle v2.2.1**

* Arbitrum address: 0xCf4EA1688bc23DD93D933edA535F8B72FC8934Ec
* Optimism address: 0xA43305Ce0164D87d7B2368f91a1dcC4eBdA75127
* Polygon PoS address: 0x2A8Ede62D0717C8C92b88639ecf603FDF31A8428
* Base address: 0x2A375567f5E13F6bd74fDa7627Df3b1Af6BfA5a6

**Note**: While the price feed is on-chain and permissionless, and tradeable by any user on WOOFi, we DO NOT guarantee the availability or accuracy of it. The price feed may become unavailable when the centralized liquidity source is in maintenance or any time the operator deems necessary. In no case, WOOFi is liable for any loss that might be caused by using the price feed by any third party.

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
