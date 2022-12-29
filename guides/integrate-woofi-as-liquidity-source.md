# \[V2] Integrate WOOFi as liquidity source

## Contracts

Two main contracts of WOOFi's swap function are as follows:

* `WooPPV2.sol`: the main swap contract that handles the logic operation, including setting the token info, calculating slippage, calculating the exchange amount and executing trades. it stores all tokens that are supported for trading. In this contract, it defines quote token and base token. Quote token is the reference token (i.e. stablecoins) in the contract and there is only one quote token. The contract can have multiple base tokens and they can be added by the strategist.&#x20;
* `WooRouterV2.sol`: the router contract that frontend users interact with. It interacts with the lower layer `WooPPV2.sol` to execute the sell logic and send back users the desired amount of tokens. This contract also implements the logic to route user orders to 3rd party aggregator (e.g. 1inch) when `WooPPV2.sol` does not have sufficient liquidity.

{% hint style="info" %}
More details about WOOFi smart contract architecture, implementation info, and addresses can be found in [Broken link](broken-reference "mention").
{% endhint %}

## Supported assets

`WooPPV2.sol` currently supports the following tokens. Quote token is the stablecoin and the rest are base tokens. Currently, WooPP V2 is only production live on Arbitrum.

{% tabs %}
{% tab title="Arbitrum" %}
| Token |                   Address                  |
| :---: | :----------------------------------------: |
|  ETH  | 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE |
|  WETH | 0x82aF49447D8a07e3bd95BD0d56f35241523fBab1 |
|  WBTC | 0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f |
|  WOO  | 0xcAFcD85D8ca7Ad1e1C6F82F651fA15E33AEfD07b |
|  USDC | 0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8 |
{% endtab %}
{% endtabs %}

## Integrate WOOFi as a liquidity source

When integrating WOOFi as a liquidity source, you can either interact with `WooRouterV2.sol` or `WooPPV2.sol`.

### Integrating `WooRouterV2.sol` (recommended)

The straightforward and recommended way is interacting with `WooRouterV2.sol` contract which provides the query and swap for any two specified tokens.

#### Router Contract Addresses

Arbitrum: [0x9aEd3A8896A85FE9a8CAc52C9B402D092B629a30](https://arbiscan.io/address/0x9aed3a8896a85fe9a8cac52c9b402d092b629a30#code)

**Interface**

```solidity
    /// @dev query the amount to swap fromToken -> toToken
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of fromToken to swap
    /// @return toAmount the predicted amount to receive
    function querySwap(
        address fromToken,
        address toToken,
        uint256 fromAmount
    ) external view returns (uint256 toAmount);
    
    /// @dev query the amount to swap fromToken -> toToken,
    ///      WITHOUT checking the reserve balance; so it
    ///      always returns the quoted amount (for reference).
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of fromToken to swap
    /// @return toAmount the predicted amount to receive
    function tryQuerySwap(
        address fromToken,
        address toToken,
        uint256 fromAmount
    ) external view returns (uint256 toAmount);
    
    /// @notice Swap `fromToken` to `toToken`.
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of `fromToken` to swap
    /// @param minToAmount the minimum amount of `toToken` to receive
    /// @param to the destination address
    /// @param rebateTo the rebate address (optional, can be 0)
    /// @return realToAmount the amount of toToken to receive
    function swap(
        address fromToken,
        address toToken,
        uint256 fromAmount,
        uint256 minToAmount,
        address payable to,
        address rebateTo
    ) external payable returns (uint256 realToAmount);
```

**Sample code**

<pre class="language-solidity"><code class="lang-solidity">// Query Part
address fromToken = 0x2f2a2543B76A4166549F7aaB2e75Bef0aefC5B0f;  // wbtc
address toToken = 0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8;    // usdc
uint256 amount1 = wooRouter.querySwap(
  fromToken, 
  toToken,
  100000000  // 1 wbtc amount in decimal 8
)

<strong>uint256 amount2 = wooRouter.tryQuerySwap(
</strong>  fromToken,
  toToken,
  100000000); // 1 wbtc amount in decimal 8

// NOTE: when the balance reserve is enough for swap, amount1 = amount2;
// otherwise, amount2 is still calculated based on wooracle price and woo market-making
// formula, but the amount1 qeury simply failed with insufficient balance error.


// Swap Part
uint256 amountIn = 100000000;
IERC20(fromToken).safeApprove(address(wooRouter), amountIn);
uint256 realToAmount = wooRouter.swap(
  fromToken,      // wbtc token address
  toToken,        // usdc token address
  amountIn,       // wbtc amount to swap in decimal 8
  19000000000,    // min amount = 19000 usdc (price: 19500)
  address(this),  // the address to receive the swap fund
  address(0)      // the rebateTo address
); </code></pre>



### Integrating `WooPPV2.sol` (more advanced)&#x20;

An alternative way to integrate directly with `WooPPV2.sol` , which would be slightly more gas efficient, but it requires writing the smart contract code and manually send the `fromToken` to `WooPPV2.sol`.

#### Contract addresses:

Arbitrum: [0xeFF23B4bE1091b53205E35f3AfCD9C7182bf3062](https://arbiscan.io/address/0xeff23b4be1091b53205e35f3afcd9c7182bf3062#code)

#### Interface

```solidity

    /// @notice The quote token address (immutable).
    /// @return address of quote token
    function quoteToken() external view returns (address);

    /// @notice Gets the pool size of the specified token (swap liquidity).
    /// @param token the token address
    /// @return the pool size
    function poolSize(address token) external view returns (uint256);

    /// @notice Query the amount to swap `fromToken` to `toToken`, without checking the pool reserve balance.
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of `fromToken` to swap
    /// @return toAmount the swapped amount of `toToken`
    function tryQuery(
        address fromToken,
        address toToken,
        uint256 fromAmount
    ) external view returns (uint256 toAmount);

    /// @notice Query the amount to swap `fromToken` to `toToken`, with checking the pool reserve balance.
    /// @dev tx reverts when 'toToken' balance is insufficient.
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of `fromToken` to swap
    /// @return toAmount the swapped amount of `toToken`
    function query(
        address fromToken,
        address toToken,
        uint256 fromAmount
    ) external view returns (uint256 toAmount);

    /// @notice Swap `fromToken` to `toToken`.
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of `fromToken` to swap
    /// @param minToAmount the minimum amount of `toToken` to receive
    /// @param to the destination address
    /// @param rebateTo the rebate address (optional, can be address ZERO)
    /// @return realToAmount the amount of toToken to receive
    function swap(
        address fromToken,
        address toToken,
        uint256 fromAmount,
        uint256 minToAmount,
        address to,
        address rebateTo
    ) external returns (uint256 realToAmount);

```

#### Sample code

```solidity
    IWooPPV2 public wooPool; // IWooPPV2 smart contrac
    
    function sampleFunc(...) external {
        ... your business code ..

        // query the trade
        address fromToken = WBTC_ADDR;
        address toToken = USDT_ADDR;
        uint256 amountIn = 10000000; // 0.1 btc
        
        uint256 amountOut = wooPool.query(fromToken, toToken, amountIn);
        // or wooPool.tryQuery if you don't need the swap.
 
        // swap
        IERC20(toToken).safeTransfer(address(wooPool), amountIn);
        uint256 realToAmount = wooPool.swap(
            fromToken,
            toToken,
            amountIn,
            amountOut * 99 / 100, 
            address(this),
            address(0)
        );
        
        ... other business code to deal with `toToken` ...
    }
    
```

## sPMM offchain simulation

For aggregators running offchain routing simulation, you can find the typescript implementation of WOOFi's sPMM algorithm in the github (to be added soon).