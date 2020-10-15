---
layout: default
title: Liquidation
has_children: false
parent: Trading Guide
nav_order: 7
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
    
-   In case the position is not fully liquidated (for example due to lack of liquidity), an off market trade for the remaining position is executed at the Bankruptcy Price between the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the remaining position. 

**When position size > Position Threshold**

When the size of the position in liquidation is greater than the [Position Threshold]({{site.baseurl}}/docs/trading-guide/margin-explainer/#risk-limits-margin-requirement-vs-position-size) Incremental Liquidation is used. Since reducing the position size lowers the margin requirement, it is possible to liquidate part of a position and ensure that the remainder of the position has sufficiently margined. Thus, Incremental Liquidation helps to avoid full liquidation of a trader's position. The steps involved in this are listed below:

-   All open orders on the contract are cancelled. This may or may not free up some margin blocked for these order.
    
- The liquidation Engine computes the minimum position size (the Liquidation Size) required to be liquidated such that the liquidation price of the remaining position is 1% away from the Mark Price. Please note that if the Liquidation Size turns our to be equal to the position size, then the entire position will be liquidated.

- The Partial Position in Liquidation (PPL) is treated as a separate position and is assumed to have pro-rata share of the position margin. The Maintenance Margin percent (MM%) for the PPL is computed Using the Liquidation Size. The Implied Bankruptcy Price of the PPL is assumed to be MM% away from the current Mark Price. An Immediate-or-Cancel order of size equal to Liquidation Size with limit price equal to the Implied Bankruptcy Price is submitted to the market. 

- If the IOC order is filled at a price better than the Implied Bankruptcy Price, a liquidation charge (equaling $$Maintenance\ Margin_{min}$$) is deducted from the position margin assigned to the PPL. Any leftover margin after this deduction is added to the position margin of the still open position.

-   In case the ICO order is not fully filled, an off market trade at the Implied Bankruptcy Price of the PPL is executed between the trader and the Liquidation Engine. This essentially results in the Liquidation Engine taking over the part of the PPL that could not be liquidated.


## Liquidation Engine

As is evident from the discussion above, the Liquidation Engine always takes over a position at its bankruptcy price. The goal of the Liquidation Engine is to close positions it acquires at price which is equal to or better than the acquisition price. To this end, as soon as the Liquidation Engine takes over a position, it places a limit order to close the position. Generally, the price of this limit order will be away from the current Mark Price  Liquidation Engine has acquired the position. Generally, this limit price would be away from Mark Price and would provide an opportunity to traders attractive entry point. However, if the Mark Price breaches the limit price, then the Liquidation Engine cancels the limit order and triggers [auto-deleveraging]({{site.baseurl}}/docs/trading-guide/ADL). 

## Liquidation Examples

For illustrative purposes, let's assume that the maintenance margin requirement for the BTCUSD perpetual contract is given by the following equation:

```
When position size < 5 BTC, MM% = 0.5%
When position size > 5 BTC, MM% = 0.5% + 0.075% * (position size - 5) 
```
**Case 1**
```
A trader is long 20000 contracts at an entry price of USD 10000 at maximum allowed leverage
Position  size = 2 BTC => MM% = 0.5%
liquidation price = 9950 & bankruptcy price = 9901
```

When Mark Price goes below 9950, this position will go into liquidation and it will be liquidated in a single shot by placing a limit IOC order to sell 20000 contracts with a limit price of 9901. In case, some part of the limit IOC order remains unfilled, the Liquidation Engine takes over the remaining position through an off market trade at 9901.

**Case 2**
```
A trade is long 200000 contracts at an entry price of USD 10000 at maximum allowed leverage
Position  size = 2 BTC => MM% = 1.63% & IM% = 3.25%
liquidation price = 9940.5 & bankruptcy price = 9685.5
```

Lets assume current Mark Price is 9840. In this situation, incremental liquidation will apply. Because of adverse price movement, the remaining position margin is sufficient to support a position of only 62611 contracts. The system will thus attempt to liquidate a long position of 137389. This is the Partial Position in Liquidation or PPL. The maintenance margin required for a position of PPL's size is 1.16%. Therefore, the liquidation price of PPL is set to 9728, i.e. 1% away from the current Mark Price of 9840. This means a sell limit IOC order with price of 9728 is sent to the order book to close PPL. 

Let's further assume that the order book currently does not have sufficient depth to fill this limit IOC order and 50000 contracts remain unfilled. In this case, the Liquidation Engine would take over the unfiled part and would acquire a long position of 50000 contracts at 9728. Recall that the Mark Price is still at 9840. Now, the Liquidation Engine will place a sell limit order at 9728 to close this recently acquired position.

With his position partially liquidated, the trader is now left with a long position of 62611 contracts. Because the position size is smaller, so is the margin requirement for it. The new liquidation price and bankruptcy price for the remaining position are 9648 and 9593 respectively. 






    

