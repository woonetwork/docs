# The math behind sPMM v1

### Synthetic Proactive Market Making (sPMM)

sPMM is the core algorithm powering the WOOFi Swap, which provides the best way to simulate a CEX order book in a DEX liquidity pool.&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

The sPMM works as follows:

WOOFi Swap gets the order book data from the centralized exchange and estimates a trade price using mid-price, spread and slippage. Define mid-price ﻿﻿﻿$$p_0$$and spread ﻿﻿﻿$$s$$:

﻿﻿﻿$$p_0=(p_{bid}+p_{ask})/2$$

$$s=(p_{ask}-p_{bid})/p_0$$

﻿If a user wants to buy the minimum amount of base tokens, the executed price will be:

﻿﻿﻿$$p=p_{ask}=p_0*(1+s/2)$$​

We defien$$\Delta Q$$ as the amount of quote token and $$\Delta B$$ as the amount of base token. If a user wants to buy more base tokens, slippage will be applied and the average buy price will be:

﻿﻿﻿$$p=\Delta Q/\Delta B=p_{ask}*(1+slippage)$$

Slippage is determined by overall market liquidity across centralized exchanges. Generally speaking, if a user trades two orders of one pair with the same USD notional value, they should have similar slippage. Assuming that quote token is a USD stable coin, we can define the liquidity coefficient $$k$$﻿﻿﻿:

﻿﻿﻿$$k=slippage/\Delta Q$$

The average buy price is:

﻿﻿﻿$$p=\Delta Q/\Delta B=p_{ask}*(1+k*\Delta Q)$$​

We can get the sell price in the same way. The final formula is:

* Basic sell function:

﻿﻿﻿$$sellBase(ΔB) = ΔQ = ΔB * p / (1+k*ΔB*p)$$​

* Basic buy function:

﻿﻿﻿$$sellQuote(ΔQ) = ΔB = ΔQ * p^{-1} / (1 + k*ΔQ)$$​

In the situation that one user sells some base tokens and another user wants to take the other side of the trade, AMMs apply the inverse of the sell base function to calculate how many quote tokens to be sent to the user, which means the liquidity provider has to pay negative slippage to the arbitrageur to bring the balance of the pool back. WOOFi Swap provides more flexibility in the rebalance strategy by adjusting the rebalance coefficient ﻿﻿﻿﻿﻿﻿$$r (0\leq r\leq 1)$$. When $$r=1$$﻿﻿﻿ the full slippage will be paid to the arbitrageur, and when ﻿﻿﻿$$r=0$$ no slippage will be paid:

* Reverse sell function:

﻿﻿﻿$$reverseSellBase(\Delta B) = \Delta B * p / (1-k*\Delta B*p*r)$$

* Basic buy function:

$$reverseSellQuote(\Delta Q) = \Delta Q * p^{-1} / (1 - k * \Delta Q * r)$$

### Custom On-chain Price Feed

Oracle providers, such as Chainlink, have become key infrastructure of the DeFi ecosystem by pushing off-chain price data on-chain. However, in order to simulate the order book from centralized exchanges without sacrificing capital efficiency, more data than just price are needed. Along with the sPMM algorithm, WOOFi Swap uses a custom on-chain price feed that contains multiple parameters i.e. mid-price $$p$$, spread $$s$$, and liquidity coefficient $$k$$. This allows WOOFi Swap to accurately reflect the order book of the centralized liquidity source and provide competitive price quotes for traders.&#x20;

The mid-price $$p$$ is updated at 0.1% price deviation to provide a much more granular price quote on-chain. The spread $$s$$ and liquidity coefficient $$k$$ are time-weighted average values using historical data, so they are updated less frequently but can be updated immediately if market volatility rises.
