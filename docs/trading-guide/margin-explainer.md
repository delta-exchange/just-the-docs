---
layout: default
title: Margin Explainer
has_children: false
parent: Margin Trading Guide
nav_order: 2
---

# Margining Explainer

1. TOC
{:toc}

## What is Margin

Margin is the collateral that you need to post when entering into a leveraged derivatives contract. The amount required to enter into a new position is referred to as **Initial Margin**. The Initial Margin for a position depends up on both the position size and the selected leverage. 

If the trade against you, the unrealised loss in your position is adjusted against the initially posted margin. When the remaining margin (Initial Margin - Unrealised Loss) goes below **Maintenance Margin**, [liquidation process]({{site.baseurl}}/docs/trading-guide/Liquidation/#liquidation) is triggered.

## Risk Limits: Margin Requirement vs. Position Size

The Initial Margin and Maintenance margin requirements for any contract are increased as the position size increases. Increasing the margin requirement helps to ensure orderly liquidation of large positions and reduce the incidences of [auto-deleveraging]({{site.baseurl}}/docs/trading-guide/ADL). 


Margin requirement remains flat till a certain position size (Position Threshold). Once position size crosses this threshold, margin requirement increases linearly with position size. 

$$ Initial\ Margin = Initial\ Margin_{min} + Slope_{IM} * (Position\ Size - Position\ Threshold )$$


\begin{equation}
  D_{it} =
    \begin{cases}
      1 & \text{if bank $i$ issues ABs at time $t$}\\
      2 & \text{if bank $i$ issues CBs at time $t$}\\
      0 & \text{otherwise}
    \end{cases}       
\end{equation}



## Isolated Margin

Delta Exchange follows an Isolated Margin approach in which margin is explicitly assigned to each position and is not shared across positions. However, cross-margin like functionalities can be acheived by enabling [auto margin top up]({{site.baseurl}}/docs/trading-guide/automargin).

A position is specific to a particular contract. Thus, for each contract, two margin sub-accounts are maintained:

- **Position Margin:** Margin allocated to all existing (may include multiple long/ short positions) in given derivatives contract

- **Order Margin:** Margin allocated to all open orders in a given derivatives contract

It is worth noting that Unrealized PnL is not factored into Position Margin or Order Margin. 

Position Margins and Order Margins are allocated from your Wallet Balance. Thus, at any time, the amount that remains unallocated is what is available for placing a new order. This is referred to as Available Balance.

$$Available\  Balance = Wallet\  Balance - (Position\  Margin + Order\  Margin)$$

Everytime a new order is placed, the system does three things:

 1. computes the Reservation margin for this new order,
 2. checks whether $$Reservation \ Margin <= Available \ Balance$$, and
 3. if $$2$$ holds, then transfer Reservation Margin amount from Available Balance to the Order Margin of the contract.

  

## Reserved Margin Computation

Initial Margin (IM) requirements for a standalone order are as follows:
-   Buy limit order

$$IM = (Initial\ Margin\% * \#Contracts * Multiplier * Limit\_Bid\_Price)$$
    
-   Buy market order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * MarkPrice)$$
    
-   Sell limit order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * Max (Limit\ Offer\ Price, BestBid)$$
    
-   Sell market order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * Max (MarkPrice, BestBid)$$
    

Now, if there are existing positions/ open orders in the contract, then the Initial Margin requirement for the new combination of position and open orders is recomputed. For this computation, positions or orders on opposite side are netted in such a manner that for two offsetting orders, margin is reserved only once.

Reserved  Margin is then the difference of the Initial Margin requirement for the combined position (existing + new order) and the Position Margin and Order Margin currently allocated to the derivative contract.

## Impact of order cancellations on margins

In a given contract, Order Margin account contains margin blocked for all the current open orders. If one or more of these open orders is cancelled, the Initial Margin requirement for the remaining open orders and existing orders is recomputed. The new Initial Margin requirement will either be same as earlier or lower. If case of latter, excess margin is released.