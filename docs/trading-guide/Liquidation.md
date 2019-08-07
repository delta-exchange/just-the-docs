---
layout: default
title: Liquidation
has_children: false
parent: Margin Trading Guide
nav_order: 6
---

# Liquidation


Each position on Delta Exchange has two associated prices:

-   **Liquidation Price:** At Liquidation Price, the difference of Position Margin minus Unrealized PnL of the position is equal to the Maintenance Margin or Liquidation Margin.
    
-   **Bankruptcy Price:** At Bankruptcy Price, the Unrealized Loss of a position equal to the Position Margin.
    
When Liquidation Price of a position is hit, liquidation of this position is initiated. It is worth noting that since Delta Exchange uses Isolated Margin, liquidation of position in a particular contract has no bearing on other existing positions and open orders on other derivative contracts.

Liquidation mechanism on Delta Exchange is comprised of the following steps:

** When position size < Position Threshold**

When the size of the position in liquidation is lower than the [Position Threshold]({{site.baseurl}}/docs/trading-guide/margin-explainer/#risk-limits-margin-requirement-vs-position-size) the entire position is liquidated in one shot. The steps involved in liquidated such a position are as follows:

-   All open orders on the contract are cancelled. This may or may not free up some margin blocked for these order.
    
-   An Immediate-or-Cancel order is submitted to the market to close the entire position in the contract. The limit price of this order is set to the Bankruptcy Price. This ensures that the Realised Loss will always be lower than the Position Margin.
    
-   If the liquidation results in being filled at a price better than the Bankruptcy Price, the remaining Position Margin is used to offset the liquidation charge. 
    
-   In case the position is not fully liquidated (for example due to lack of liquidity), an off market trade is executed at bankruptcy price betweent the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the remaining position. 

** When position size > Position Threshold**

When the size of the position in liquidation is greater than the [Position Threshold]({{site.baseurl}}/docs/trading-guide/margin-explainer/#risk-limits-margin-requirement-vs-position-size) liquidation happens in incremental fashion. Reduction in position size leads to reduction in Maintenance Margin and thus makes incremental liquidation possible. The steps involved in incremental liquidation are as follows:

-   All open orders on the contract are cancelled. This may or may not free up some margin blocked for these order.
    
- The liquidatin Engine computes the minimum position size (the Liquidation Size) required to be liquidated such that the liquidation price of the remaining position is 1% away from the Mark Price. Please note that if the Liquidation Size turns our to be equal to the position size, then the entire position will be liquidated.

- The partial position in liquidation is treated as a separate position and is assumed to have pro-rata share of the position margin. Using the Liquidation Size first the Maintenance Margin for this position is computed. Next, the Implied Bankruptcy Price of this position is assumed to be at Maintenance-Margin percent away from the current Mark Price. An Immediate-or-Cancel order of size equal to Liquidation Size with limit price equal to the implied bankruptcy price is submitted to the market. 

- If the IOC order is filled at a price better than the Implied Bankruptcy Price, a liquidation charge (computed as the Maintenance Margin required for a position of Liquidation Size assuming margin requirement does not increase with position size) is deducted from the position margin assigned to the partial position in liquidation. Any leftover margin after this deduction is added to the position margin of the still open position.

-   In case the ICO order is not fully filled (for example due to lack of liquidity), an off market trade is executed at bankruptcy price betweent the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the part of the position that could not be liquidated.


## Liquidation Engine

As soon as the Liquidation Engine acquires a position it places a limit order to close this position. The price of this limit order is set to the price at which the Liquidation Engine has acquired the position. Generally, this limit price would be away from Mark Price and would provide an opportunity to traders attractive entry point. However, if the Mark Price breaches the limit price, then the Liquidation Engine cancels the limit order and triggers [auto-deleveraging]({{site.baseurl}}/docs/trading-guide/ADL). 

    

