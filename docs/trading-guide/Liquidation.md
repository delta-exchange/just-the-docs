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
    
A position goes into liquidation when [Mark Price](https://www.delta.exchange/user-guide/docs/trading-guide/fair-price/) reaches the Liquidation Price of the position. It is worth noting that since Delta Exchange uses Isolated Margin, liquidation of position in a particular contract has no bearing on other existing positions and open orders on other derivative contracts.

Liquidation mechanism on Delta Exchange is comprised of the following steps:

**When position size < Position Threshold**

When the size of the position in liquidation is lower than the [Position Threshold]({{site.baseurl}}/docs/trading-guide/margin-explainer/#risk-limits-margin-requirement-vs-position-size) the entire position is liquidated in one shot. The steps involved in liquidated such a position are as follows:

-   All open orders on the contract are cancelled. This may or may not free up some margin blocked for these order.
    
-   An Immediate-or-Cancel (IOC) order is submitted to the market to close the entire position. The limit price of this order is set to the Bankruptcy Price. This ensures that the Realised Loss will always be lower than or equal to the Position Margin.
    
-   If the liquidation results in being filled at a price better than the Bankruptcy Price, the remaining Position Margin is used to offset the liquidation charge. 
    
-   In case the position is not fully liquidated (for example due to lack of liquidity), an off market trade for the remaining position is executed at the Bankruptcy Price betweent the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the remaining position. 

**When position size > Position Threshold**

When the size of the position in liquidation is greater than the [Position Threshold]({{site.baseurl}}/docs/trading-guide/margin-explainer/#risk-limits-margin-requirement-vs-position-size) Incremental Liquidation is used. Since reducing the position size lowers the margin reqiurement, it is possible to liquidate part of a position and ensure that the remainder of the position has sufficiently margined. Thus, Incremental Liquidation helps to avoid full liquidation of a trader's position. The steps involved in this are listed below:

-   All open orders on the contract are cancelled. This may or may not free up some margin blocked for these order.
    
- The liquidatin Engine computes the minimum position size (the Liquidation Size) required to be liquidated such that the liquidation price of the remaining position is 1% away from the Mark Price. Please note that if the Liquidation Size turns our to be equal to the position size, then the entire position will be liquidated.

- The Partial Position in Liquidation (PPL) is treated as a separate position and is assumed to have pro-rata share of the position margin. The Maintenence Margin percent (MM%) for the PPL is computed Using the Liquidation Size. The Implied Bankruptcy Price of the PPL is assumed to be MM% away from the current Mark Price. An Immediate-or-Cancel order of size equal to Liquidation Size with limit price equal to the Implied Bankruptcy Price is submitted to the market. 

- If the IOC order is filled at a price better than the Implied Bankruptcy Price, a liquidation charge (eqalling $$Maintenance\ Margin_{min}$$) is deducted from the position margin assigned to the PPL. Any leftover margin after this deduction is added to the position margin of the still open position.

-   In case the ICO order is not fully filled, an off market trade at the Implied Bankruptcy Price of the PPL is executed betweent the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the part of the PPL that could not be liquidated.


## Liquidation Engine

The Liquidation Engine always aims to close any position it acquires at a price that is equal to or better than the price at which it acquired the position. This is done by placing a limit order to close the position. Generally, the price of this limit order will be away from the current Mark Price  Liquidation Engine has acquired the position. Generally, this limit price would be away from Mark Price and would provide an opportunity to traders attractive entry point. However, if the Mark Price breaches the limit price, then the Liquidation Engine cancels the limit order and triggers [auto-deleveraging]({{site.baseurl}}/docs/trading-guide/ADL). 




    

