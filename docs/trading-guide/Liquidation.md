---
layout: default
title: Liquidation
has_children: false
parent: Margin Trading Guide
nav_order: 4
---

# Liquidtion

1. TOC
{:toc}

Each position on Delta has an associated prices:

-   Liquidation Price: At Liquidation Price, the difference of Position Margin minus Unrealized PnL of the position is equal to the Liquidation Margin.
    
-   Bankruptcy Price: At Bankruptcy Price, the Unrealized Loss of a position equal to the Position Margin.
    
When Liquidation Price of a position is hit, liquidation of this position is initiated. It is worth noting that since Delta Exchange uses Segregated Margins, liquidation of position in a particular contract has no bearing on other existing positions and open orders on other derivative contracts.

Liquidation mechanism on Delta Exchange is comprised of the following steps:

-   All open orders on the derivative contract are cancelled. This may or may not free up some margin blocked for these order.
    
-   An Immediate-or-Cancel order is submitted to the market to close the positions in the contract. The limit price of this order is set to the Bankruptcy Price. This ensures that the Realised Loss will always be lower than the Position Margin.
    
-   If the liquidation results in being filled at price better than the Bankruptcy Price, then the user keeps whatever remaining Position Margin there is.
    
-   In case the position is not fully liquidated (for example due to lack of liquidity), the remainder of the position is terminated at Bankruptcy Price and ADL is triggered.
    

**Liquidation Examples**
You have an open long position of $$1000$$ contracts in Bitcoin-Dollar Futures with an Average Entry Price of $$10000$$. The Liquidation Price for this position is $$9500$$ and the Bankruptcy Price is $$9200$$.

As soon as the Mark prices reaches $$9500$$ or below, your position will enter Liquidation. An immediate-or-cancel sell order is submitted on your behalf at $$9200$$. The limit price of this order assures that if the order is matched, the portfolio value of the margin account will not be less than zero.

**Case 1:** Liquidation order gets filled at $$9400$$. At this price since your margin is not fully eroded, the remaining margin is released.

**Case 2:** The Liquidation order at gets only partially filled and say 300 contracts are left over. This will trigger [ADL](#auto-deleveraging). The system will find counterparties on the short side and a trade between you and these counterparties for $$300$$ contracts at $$9200$$  (i.e. the Bankruptcy Price) is made to happen.