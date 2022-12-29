# WOOFi Contracts

### [Architecture](./#architecture)

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Descriptions

1. `WooRouter.sol` is the router contract. Users will only need to approve this contract to spend their ERC20 tokens. Similar to Uniswap, approval is required to interact with any trading pair on this contract. Router contract will interact with the lower layer `WooPP.sol` to execute the sell logic and send back users the desired amount of tokens.&#x20;
2. `WooPP.sol` is the basic contract that handles the logic operation of the DEX, including setting the token info, calculating slippage, calculating the exchange amount and so on. This contract stores all tokens that are supported by trading on the WOOFi Swap. In this contract, it defines quote token and base token. Quote token is the reference token in the contract and there is only one quote token. The contract can have multiple base tokens and they can be added by the strategist.&#x20;
3. `Wooracle.sol` is an oracle contract managed and developed by WOO Network team. It uploads the latest trading price, middle price and market spread information from WOO Network to the oracle contract.
4. `WooGuardian.sol` is the smart contract that is used to perform final checkup on the trade execution. The smart contract does not process funds. It provides the tolerance threshold of prices by comparing multiple oracles. For the swap amounts and quoted prices, the functions (_i.e._, `checkSwapAmount()` and `checkSwapPrice()`) in this smart contract are used to check if the swapping amounts and prices are within the thresholds or not.
5. `WooFeeManager.sol` collects swap fees of transactions processed in WOOFi's sPMM liquidity pool and distribute to various pools
6. `WooRebateManager.sol` collects and distributes rebates to broker addresses (if any)&#x20;
7. `WooVaultManager.sol` collects the fee allocated to vaults and executes WOO buy back before distributing to vaults
8. `WooStakingVault.sol` is the smart contract to allow users to stake WOO token and receive xWOO token. It also manages the unstaking period and penalty
9. `WooAccessManager.sol` manages the access of WOOFi smart contracts
10. `SuperChargerVault.sol` is the WOOFi Earn Supercharger smart contract that users deposit assets into
11. `LendingManager.sol` is the smart contract that the WOOFi LP interacts with to borrow, repay loans and set borrowing interests
12. `WithdrawManager.sol` stores the funds that users request for withdrawal after the settlement process
13. `YieldFarmingVault.sol` is the vault that stores assets deployed for external yield farming
14. `YieldFarmingStrategy.sol` manages the external yield farming auto-compounding strategy

### Smart Contract Addresses

* ****[**BNB Chain deployment**](bnb-chain-deployment.md)****
* ****[**Avalanche deployment**](avalanche-deployment.md)****
* ****[**Fantom deployment**](fantom-deployment.md)****
* ****[**Polygon deployment**](polygon-deployment.md)****
* ****[**Arbitrum deployment**](arbitrum-deployment.md)****
