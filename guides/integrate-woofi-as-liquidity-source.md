# Integrate WOOFi as a liquidity source

## Contracts

Two main contracts of WOOFi's swap function are as follows:

* `WooPPV2.sol`: the main swap contract that handles the logic operation, including setting the token info, calculating slippage, calculating the exchange amount and executing trades. it stores all tokens that are supported for trading. In this contract, it defines quote token and base token. Quote token is the reference token (i.e. stablecoins) in the contract and there is only one quote token. The contract can have multiple base tokens and they can be added by the strategist.&#x20;
* `WooRouterV2.sol`: the router contract that frontend users interact with. It interacts with the lower layer `WooPPV2.sol` to execute the sell logic and send back users the desired amount of tokens. This contract also implements the logic to route user orders to 3rd party aggregator (e.g. 1inch) when `WooPPV2.sol` does not have sufficient liquidity.

## Supported assets

**Adding `IntegrationHelper.sol`**

`WooPPV2.sol` has one quote token which is typically the dominant stablecoin of each chain and multiple base tokens. A `IntegrationHelper.sol` contract is deployed on each supported chain which contains the address of the quote token and the list of tradable base token addresses in `WooPPV2.sol.`

The latest list of supported tokens can be retrieved via the `getSupportTokens()` function which will return two values:

* first value: quoteToken address
* second value: array of baseToken addresses

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### Contract addresses:

Arbitrum: [0x28D2B949024FE50627f1EbC5f0Ca3Ca721148E40](https://arbiscan.io/address/0x28D2B949024FE50627f1EbC5f0Ca3Ca721148E40#readContract)

Avalanche: [0x020630613E296c3E9b06186f630D1bF97A2B6Ad1](https://snowtrace.io/address/0x020630613E296c3E9b06186f630D1bF97A2B6Ad1#readContract)

BSC: [0xAA9c15cd603428cA8ddD45e933F8EfE3Afbcc173](https://bscscan.com/address/0xAA9c15cd603428cA8ddD45e933F8EfE3Afbcc173)

Optimism - [0x96329d66074EB8386Ae8bFD6698B2E3FDA87e15E](https://optimistic.etherscan.io/address/0x96329d66074EB8386Ae8bFD6698B2E3FDA87e15E#readContract)

Polygon POS - [0x7Ba560eB735AbDCf9a3a5692272652A0cc81850d](https://polygonscan.com/address/0x7Ba560eB735AbDCf9a3a5692272652A0cc81850d#readContract)

zkSync Era - [0x636DfeB023463F176f87D61E3B604231986bd935](https://explorer.zksync.io/address/0x636DfeB023463F176f87D61E3B604231986bd935#contract)

Polygon zkEVM - [0x044c08639bD59BEB4F6ec52c0da6CD47283534E8](https://zkevm.polygonscan.com/address/0x044c08639bD59BEB4F6ec52c0da6CD47283534E8)

Linea - [0x7e1996945eA8866DE873179DC1677E93A4380107](https://lineascan.build/address/0x7e1996945ea8866de873179dc1677e93a4380107#readContract)

Base - [0xC4E9B633685461E7B7A807D12a246C81f96F31B8](https://basescan.org/address/0xC4E9B633685461E7B7A807D12a246C81f96F31B8)

Mantle - [0x86b223E83D2FA43456b433687c8F47A35a9bE24C](https://explorer.mantle.xyz/address/0x86b223E83D2FA43456b433687c8F47A35a9bE24C)

## Integrate WOOFi as a liquidity source

When integrating WOOFi as a liquidity source, you can either interact with `WooRouterV2.sol` or `WooPPV2.sol`.

### Integrating `WooPPV2.sol` &#x20;

Another way is to integrate directly with `WooPPV2.sol`. This approach is slightly more gas efficient, but it requires writing the smart contract code and manually send the `fromToken` to `WooPPV2.sol`, which a better choice for apps that already have an aggregation logic.&#x20;

#### Contract addresses:

Arbitrum: [0xeFF23B4bE1091b53205E35f3AfCD9C7182bf3062](https://arbiscan.io/address/0xeff23b4be1091b53205e35f3afcd9c7182bf3062#code)

Avalanche: [0x3b3E4b4741e91aF52d0e9ad8660573E951c88524](https://snowtrace.io/address/0x3b3e4b4741e91af52d0e9ad8660573e951c88524#code)

BSC: [0x59dE3B49314Bf5067719364A2Cb43e8525ab93FA](https://bscscan.com/address/0x59de3b49314bf5067719364a2cb43e8525ab93fa)

Optimism - [0xd1778F9DF3eee5473A9640f13682e3846f61fEbC](https://optimistic.etherscan.io/address/0xd1778f9df3eee5473a9640f13682e3846f61febc#code)

Polygon POS - [0x7081A38158BD050Ae4a86e38E0225Bc281887d7E](https://polygonscan.com/address/0x7081a38158bd050ae4a86e38e0225bc281887d7e#code)

zkSync Era - [0x42ED123EB5266A5B8E2B54B2C76180CCF5e72FEe](https://explorer.zksync.io/address/0x42ED123EB5266A5B8E2B54B2C76180CCF5e72FEe#contract)

Polygon zkEVM - [0xF5d215d9C84778F85746D15762DaF39B9E83a2d6](https://zkevm.polygonscan.com/address/0xF5d215d9C84778F85746D15762DaF39B9E83a2d6)

Linea - [0xF5d215d9C84778F85746D15762DaF39B9E83a2d6](https://lineascan.build/address/0xf5d215d9c84778f85746d15762daf39b9e83a2d6)

Base - [0xb130a49065178465931d4f887056328CeA5D723f](https://basescan.org/address/0xb130a49065178465931d4f887056328CeA5D723f)

Mantle - [0x9D1A92e601db0901e69bd810029F2C14bCCA3128](https://explorer.mantle.xyz/address/0x9D1A92e601db0901e69bd810029F2C14bCCA3128)

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

#### sPMM offchain simulation

For aggregators running offchain routing simulation, you can find the typescript implementation of WOOFi's sPMM algorithm in the github (to be added soon).

### Integrating `WooRouterV2.sol`

The straightforward way is interacting with `WooRouterV2.sol` contract which provides the query and swap for any two specified tokens. It streamlined the logic of swapping native blockchain coin and swapping between any of the two supported assets, which simplifies the integration for apps that do not have an existing aggregation logic.&#x20;

#### Contract addresses:

Same address across Arbitrum, Avalanche, BSC, Optimism, Polygon PoS, Ethereum, Linea, Base, Mantle - 0x4c4AF8DBc524681930a27b2F1Af5bcC8062E6fB7

zkSync: 0x09873bfECA34F1Acd0a7e55cDA591f05d8a75369

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
); 
</code></pre>
