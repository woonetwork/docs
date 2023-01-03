# \[v1] sPMM liquidity integration

## Contracts

Two main contracts of WOOFi's swap function are as follows:

* `WooPP.sol`: the main swap contract that handles the logic operation, including setting the token info, calculating slippage, calculating the exchange amount and executing trades. it stores all tokens that are supported for trading. In this contract, it defines quote token and base token. Quote token is the reference token (i.e. stablecoins) in the contract and there is only one quote token. The contract can have multiple base tokens and they can be added by the strategist.&#x20;
* `WooRouter.sol`: the router contract that frontend users interact with. It interacts with the lower layer `WooPP.sol` to execute the sell logic and send back users the desired amount of tokens. This contract also implements the logic to route user orders to 3rd party aggregator (e.g. 1inch) when `WooPP.sol` does not have sufficient liquidity.

{% hint style="info" %}
More details about WOOFi smart contract architecture, implementation info, and addresses can be found in [Broken link](broken-reference "mention").
{% endhint %}

## Supported assets

`WooPP.sol` currently supports the following tokens. Quote token is the stablecoin and the rest are base tokens.&#x20;

{% tabs %}
{% tab title="BNB Chain" %}
| Token |                   Address                  |
| :---: | :----------------------------------------: |
|  BNB  | 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE |
|  WBNB | 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c |
|  BTCB | 0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c |
|  ETH  | 0x2170ed0880ac9a755fd29b2688956bd959f933f8 |
|  WOO  | 0x4691937a7508860f876c9c0a2a617e7d9e945d4b |
|  USDT | 0x55d398326f99059fF775485246999027B3197955 |
{% endtab %}

{% tab title="Avalanche" %}
| Token  | Address                                    |
| ------ | ------------------------------------------ |
| AVAX   | 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE |
| WAVAX  | 0xB31f66AA3C1e785363F0875A1B74E27b85FD66c7 |
| BTC.b  | 0x152b9d0FdC40C096757F570A51E494bd4b943E50 |
| WETH.e | 0x49D5c2BdFfac6CE2BFdB6640F4F80f226bc10bAB |
| WOO.e  | 0xaBC9547B534519fF73921b1FBA6E672b5f58D083 |
| USDC   | 0xB97EF9Ef8734C71904D8002F8b6Bc66Dd9c48a6E |
{% endtab %}

{% tab title="Fantom" %}
| Token | Address                                    |
| ----- | ------------------------------------------ |
| FTM   | 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE |
| WFTM  | 0x21be370D5312f44cB42ce377BC9b8a0cEF1A4C83 |
| BTC   | 0x321162Cd933E2Be498Cd2267a90534A804051b11 |
| ETH   | 0x74b23882a30290451A17c44f4F05243b6b58C76d |
| WOO   | 0x6626c47c00F1D87902fc13EECfaC3ed06D5E8D8a |
| USDC  | 0x04068DA6C83AFCFA0e13ba15A6696662335D5B75 |
{% endtab %}

{% tab title="Polygon" %}
| Token  | Address                                    |
| ------ | ------------------------------------------ |
| MATIC  | 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE |
| WMATIC | 0x0d500B1d8E8eF31E21C99d1Db9A6444d3ADf1270 |
| BTC    | 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6 |
| ETH    | 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619 |
| WOO    | 0x1B815d120B3eF02039Ee11dC2d33DE7aA4a8C603 |
| USDC   | 0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174 |
{% endtab %}
{% endtabs %}

## Integrate WOOFi as a liquidity source

When integrating WOOFi as a liquidity source, you can either interact directly with `WooPP.sol` or through`WooRouter.sol`.&#x20;

### Integrating `WooPP.sol` (recommended)&#x20;

For aggregators or trading bots, we recommend integrate directly with `WooPP.sol` to be more gas efficient.

#### Contract addresses:

BNB Chain: [0xbf365Ce9cFcb2d5855521985E351bA3bcf77FD3F](https://bscscan.com/address/0xbf365Ce9cFcb2d5855521985E351bA3bcf77FD3F)

Avalanche:  [0x1df3009c57a8B143c6246149F00B090Bce3b8f88](https://snowtrace.io/address/0x1df3009c57a8B143c6246149F00B090Bce3b8f88#code)

Fantom: [0x9503E7517D3C5bc4f9E4A1c6AE4f8B33AC2546f2](https://ftmscan.com/address/0x9503e7517d3c5bc4f9e4a1c6ae4f8b33ac2546f2#code)

Polygon: [0x7400B665C8f4f3a951a99f1ee9872efb8778723d](https://polygonscan.com/address/0x7400b665c8f4f3a951a99f1ee9872efb8778723d#code)

#### Interface:

```
    /// @dev get the quote token address (immutable)
    /// @return address of quote token
    function quoteToken() external view returns (address);

    /// @dev Query the amount for selling the base token amount.
    /// @param baseToken the base token to sell
    /// @param baseAmount the amount to sell
    /// @return quoteAmount the swapped quote amount
    function querySellBase(address baseToken, uint256 baseAmount) external view returns (uint256 quoteAmount);

    /// @dev Query the amount for selling the quote token.
    /// @param baseToken the base token to receive (buy)
    /// @param quoteAmount the amount to sell
    /// @return baseAmount the swapped base token amount
    function querySellQuote(address baseToken, uint256 quoteAmount) external view returns (uint256 baseAmount);

    /// @dev Swap baseToken into quoteToken
    /// @param baseToken the base token
    /// @param baseAmount amount of baseToken that user want to swap
    /// @param minQuoteAmount minimum amount of quoteToken that user accept to receive
    /// @param to quoteToken receiver address
    /// @param rebateTo the wallet address for rebate
    /// @return quoteAmount the swapped amount of quote token
    function sellBase(
        address baseToken,
        uint256 baseAmount,
        uint256 minQuoteAmount,
        address to,
        address rebateTo
    ) external returns (uint256 quoteAmount);

    /// @dev Swap quoteToken into baseToken
    /// @param baseToken the base token
    /// @param quoteAmount amount of quoteToken that user want to swap
    /// @param minBaseAmount minimum amount of baseToken that user accept to receive
    /// @param to baseToken receiver address
    /// @param rebateTo the wallet address for rebate
    /// @return baseAmount the swapped amount of base token
    function sellQuote(
        address baseToken,
        uint256 quoteAmount,
        uint256 minBaseAmount,
        address to,
        address rebateTo
    ) external returns (uint256 baseAmount);
```

#### Query & swap functions

```
    address public immutable pool; // IWooPP smart contract
    
    function query(
        uint _amountIn, 
        address _tokenIn, 
        address _tokenOut
    ) internal override view returns (uint256 amountOut) {
        if (_amountIn == 0) { 
            return 0; 
        }
        if (_tokenIn == quoteToken) {
            amountOut = IWooPP(pool).querySellQuote(_tokenOut, _amountIn);
        } else if (_tokenOut == quoteToken) {
            amountOut = IWooPP(pool).querySellBase(_tokenIn, _amountIn);
        } else {
            uint quoteAmount = IWooPP(pool).querySellBase(_tokenIn, _amountIn);
            amountOut = IWooPP(pool).querySellQuote(_tokenOut, quoteAmount);
        }
    }

    function swap(
        uint _amountIn, 
        uint _amountOut, 
        address _tokenIn, 
        address _tokenOut, 
        address _to
    ) external returns (uint256 realToAmount) {
        // check parameters and approve the allowrance if needed
    
        if (_tokenIn == quoteToken) {
            // case 1: quoteToken --> baseToken
            realToAmount = IWooPP(pool).sellQuote(
                _tokenOut,
                _amountIn,
                _amountOut,
                _to,
                address(0)
            );
        } else if (_tokenOut == quoteToken) {
            // case 2: fromToken --> quoteToken
            realToAmount = IWooPP(pool).sellBase(
                _tokenIn, 
                _amountIn, 
                _amountOut, 
                _to, 
                address(0)
            );
        } else {
            // case 3: fromToken --> quoteToken --> toToken
            uint256 quoteAmount = IWooPP(pool).sellBase(
                _tokenIn, 
                _amountIn, 
                0, 
                address(this), 
                address(0)
            );
            _approveIfNeeded(quoteToken, quoteAmount);
            realToAmount = IWooPP(pool).sellQuote(
                _tokenOut, 
                quoteAmount, 
                _amountOut, 
                _to, 
                address(0)
            );
        }

        // emit events if needed
    }
```

#### Sample integration code (Yield Yak)&#x20;

[https://snowtrace.io/address/0xa64c5c58fc1510de3ff2ee644e030d666b660ea6#code](https://snowtrace.io/address/0xa64c5c58fc1510de3ff2ee644e030d666b660ea6#code)&#x20;

### Integrating `WooRouter.sol`&#x20;

A simpler way is calling `WooRouter.sol` contract which provides the similar query and swap as above. This way costs slightly more gas than directly integrating with `WooPP.sol`.

#### Contract addresses

BNB Chain: [0xcef5be73ae943b77f9bc08859367d923c030a269](https://bscscan.com/address/0xcef5be73ae943b77f9bc08859367d923c030a269#code)

Avalanche: [0x5AA6a4E96A9129562e2fc06660D07FEdDAAf7854](https://snowtrace.io/address/0x5AA6a4E96A9129562e2fc06660D07FEdDAAf7854#code)

Fantom: [0x37b5a5a730dad670874f26cc5507bb1b9705e447](https://ftmscan.com/address/0x37b5a5a730dad670874f26cc5507bb1b9705e447#code)

Polygon: [0x9D1A92e601db0901e69bd810029F2C14bCCA3128](https://polygonscan.com/address/0x9d1a92e601db0901e69bd810029f2c14bcca3128#code)

**Query function**

```solidity
    /// @dev query the amount to swap fromToken -> toToken
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of fromToken to swap
    /// @return toAmount the swapped amount to receive
    function querySwap(
        address fromToken,
        address toToken,
        uint256 fromAmount
    ) external view returns (uint256 toAmount);
```

Sample code to retrieve quote on selling 1 BTC:

```solidity
wooRouter.querySwap(
  0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c, // btcb token address
  0x55d398326f99059fF775485246999027B3197955, // usdt token address
  1000000000000000000);                       // btcb amount to swap in decimal 18
```

**Swap function**

```solidity
    /// @dev swap fromToken -> toToken
    /// @param fromToken the from token
    /// @param toToken the to token
    /// @param fromAmount the amount of fromToken to swap
    /// @param minToAmount the minimum amount of toToken required or the tx reverts
    /// @param to the destination address
    /// @param rebateTo the address to receive rebate (rebate rate is set to 0 now)
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

Sample code to swap 1 BTC to USDT:

```solidity
wooRouter.swap(
  0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c, // btcb token address
  0x55d398326f99059fF775485246999027B3197955, // usdt token address
  1000000000000000000,                        // btcb amount to swap in decimal 18
   990000000000000000,                        // min amount for 1% slippage
  0xd51062A4aF7B76ee0b2893Ef8b52aCC155393E3D, // the address to receive the swap fund
  0);                                         // the rebate address

```

## sPMM offchain simulation

For aggregators running offchain routing simulation, you can find the typescript implementation of WOOFi's sPMM algorithm in the [github](https://github.com/woonetwork/woofi\_swap\_smart\_contracts/tree/main/integration).&#x20;
