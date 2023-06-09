# Become a WOOFi broker

### What are WOOFi brokers?

WOOFi's bespoken sPMM algorithm provides deep liquidity and MEV proof trade execution on assets such as BTC, ETH, BNB, AVAX, FTM and predominant stablecoins on each blockchain. We aim to be the liquidity layer across DeFi, Web3 and GameFi spaces.

WOOFi has the lowest swap fee in the market with only 2.5bps on each transaction, in which 0.02% is used to buy back WOO tokens and reward WOO onchain stakers. WOOFi brokers are the applications or sites that route trades to WOOFi smart contracts. A whitelisted broker is eligible to receive 0.5bps of each trade that is sent to WOOFi via the broker's application as rebates in the form of quote tokens i.e. stablecoins.

### How to enroll as a broker to receive rebates?

Any application such as DEX aggregators, wallets, web3 apps or websites that integrate WOOFi's sPMM liquidity can enroll as a broker to receive rebates. Fill out the enrollment form below and we will get in touch with you shortly.

:point\_right: [**Enrollment Form**](https://docs.google.com/forms/d/e/1FAIpQLSdTTfytZmcKLwGTl6MUxGiaou1gAXhd83roAGgNCiIkBeYtFQ/viewform?usp=sf\_link)

Once the enrollment information is reviewed, our team will get in contact with you and the provided broker rebate address will be added to the whitelist in `WooRebateManager.sol` on corresponding blockchains.

In order to receive rebates, the broker needs to integrate WOOFi as a liquidity source following [integration docs](integrate-woofi-as-liquidity-source.md). For a trade to be eligible for rebates, the whitelisted rebate address must be included in the field `address rebateTo` in the transaction that is sent to WOOFi smart contracts.

### How to check and claim the rebates?

The rebate is processed on a daily basis via smart contracts, the broker can check the eligible rebates and claim anytime you want on the chain that WOOFi liquidity is integrated.

<table><thead><tr><th width="170">Network</th><th>Rebate manager smart contract address</th></tr></thead><tbody><tr><td>Arbitrum</td><td><a href="https://arbiscan.io/address/0x505ac728645d2ef84380961f72baea500b3efa3f#code">0x505ac728645d2ef84380961F72bAea500b3efa3f</a></td></tr><tr><td>Avalanche</td><td><a href="https://snowtrace.io/address/0x6cb1bc6c8aabdae822a2bf8d83b36291cb70f169#code">0x49d26A6Eeeb49E79a0C73B95fD99D23698D3614A</a></td></tr><tr><td>BNB Chain</td><td><a href="https://bscscan.com/address/0xce7cdc8e5c00796392e611d95c713420a6e31342#code">0xce7CDc8e5C00796392E611D95C713420A6e31342</a></td></tr><tr><td>Fantom</td><td><a href="https://ftmscan.com/address/0x6f09ae4925739453d7c8c9a22fd07585148dfc01#code">0x6F09AE4925739453d7C8c9a22fD07585148DFc01</a></td></tr><tr><td>Optimism</td><td><a href="https://optimistic.etherscan.io/address/0x36b680fb76dad86bcb2cefc83fae05e3fe147706#code">0x36b680fB76Dad86bcB2Cefc83fAE05e3Fe147706</a></td></tr><tr><td>Polygon</td><td><a href="https://polygonscan.com/address/0x913e116cd0e279763b0419798c0ba18f9311b390#code">0x913E116cD0E279763B0419798c0bA18F9311B390</a></td></tr></tbody></table>

The `WooRebateManager` smart contracts on the following chains have been replaced. If you were a broker on these chains before Jan 11 2023, please withdraw rebates from the following addresses asap.

<table><thead><tr><th width="184">Network</th><th>Deprecated rebate manager smart contract address</th></tr></thead><tbody><tr><td>Avalanche</td><td><a href="https://snowtrace.io/address/0xbf401edbb0265d037bfc89dffeb7dbda10b57c22#readContract">0xbF401edBb0265D037BfC89DfFeB7dbDa10b57C22</a></td></tr><tr><td>BNB Chain</td><td><a href="https://bscscan.com/address/0x0208d735576b3d974024237393f4617285bf0563#readContract">0x0208D735576B3D974024237393F4617285bf0563</a><br><a href="https://bscscan.com/address/0x4f3e0b02f50937bc21f523f1dd430a34caf4de98#code">0x4f3e0B02F50937Bc21F523F1Dd430A34cAF4de98</a></td></tr><tr><td>Fantom</td><td><a href="https://ftmscan.com/address/0x0d37e768c3ed8a3fa494d03af69605c5be9a68e4#readContract">0x0D37e768c3eD8a3FA494d03AF69605C5bE9a68e4</a></td></tr><tr><td>Polygon</td><td><a href="https://polygonscan.com/address/0x3d605fa64595dba86f7780e128816adaaeca8a2b#code">0x3d605fA64595Dba86f7780E128816ADAAeCA8A2b</a></td></tr></tbody></table>

#### Check eligible rebates

* Step 1 - go to the `WooRebateManager` smart contract address using the corresponding blockchain explorer (click the link above)
* Step 2 - click 'Read Contract' under the "Contract" tab

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

* Step 3 - go to the 'pendingRebate' function, input your whitelisted rebate address and click 'Query', and you will see the current amount of claimable rebates in the quote token. i.e. stablecoins

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Claim rebates

* Step 1 - visit the `WooRebateManager` smart contract address via the blockchain explorer and click the 'Write Contract' under the 'Contract' tab.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* Step 2 - click 'Connect to web3' and sign in with your whitelisted rebate address.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* Step 3 - Claim your pending rebate by clicking the 'Write' button under the 'claimRebate' function, sign the transaction in your wallet to receive the rebates.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

**Note:** With respect to the rebates provided to WOOFi brokers, WOO Network reserves the right to modify, suspend or discontinue any and all of the rules in the program at any time without the consensus of WOOFi brokers.
