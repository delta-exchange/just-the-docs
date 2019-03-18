---
layout: default
title: Fair Price Marking
has_children: false
parent: Margin Trading Guide
nav_order: 1
permalink: /docs/trading-guide/fair-price-marking
---

# Fair Price Marking

Leverage is inherent in derivative contracts, combined with high volatility of crypto-currencies can lead to unwarranted liquidations. Lack of liquidity could further exacerbate this situation as price swings in a derivatives contract relative to the underlying index could widen even further.

To ensure a great trader experience, Delta marks all open position at Fair Price instead of Last Traded Price. The Fair Price has lesser volatility and is more resilient against attempts to manipulate market.

It is worth noting that Fair Price marking is relevant only computation of Unrealized PnL and Liquidation price. Realized PnL is computed using actual trading prices and is thus not impacted by Fair Price.

## Calculation of Fair Price of a Futures Contract

The price of a futures contract converges to the underlying index price at the time of contract maturity, i.e.

$$Futures\_Price = Underlying\_Index Price$$

At all other times, the price of a futures contract broadly moves in tandem with the price of the Underlying Index, with the difference between the two referred to as basis, i.e.

$$Basis = Futures\_Price - Underlying\_Index\_Price$$

Since the Underlying Index is the foundation of the futures contract, it is logical to assume that

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

The $$Underlying\_Index\_Price$$ is obviously independent of the trading happening on Delta Exchange and is sourced in real-time from leading spot exchanges.

## Impact Price

To understand the computation of $$Fair\_Basis$$, we first need to introduce the notion of Impact Price. This price tries to estimate the price at which a typical long or short position (called Impact Position) in the futures contract can be entered at any given time.

Impact Position, in terms of number is contracts to be traded, is provided in the specifications for each futures contract. It is easy to see that Impact Price is a function of: (a) Impact Position and (b) current state of the order book.

$$Impact\_Bid\_Price = \text{Average fill price to execute a typical short trade}$$

$$Impact\_Ask\_Price = \text{Average fill price to execute a typical long trade}$$

$$Impact\_Mid\_Price = \text{Average of } Impact\_Bid\_Price \text{ and } Impact\_Ask\_Price$$
 

  
## Fair Basis Calculation

We first compute an annualised fair value basis rate, $$\%Fair\_Basis$$:

$$\%Fair\_Basis = (Impact\_Mid\_Price/ Underlying\_Index\_Price - 1) * (365*86400/ time\_to\_expiry\_in\_sec)$$

$$\%Fair\_Basis$$ is computed only once every minute. Further, in case at any time of update, market is illiquid, i.e. 

$$(Impact\_Ask\_Price - Impact\_Bid\_Price) > Maintenance\_Margin$$ 

$$\%Fair\_Basis$$ is not updated.

$$\text{Now, } Fair\_Basis = Underlying\_Index\_Price * \%Fair\_Basis * (time\_to\_expiry\_in\_sec/ (365* 86400))$$

Now, that we have the $$Fair\_Basis$$, the fair price of the futures (i.e. the mark price) can be easily computed:

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

It is worth noting that only live positions are marked using the Fair Price. Thus, unrealised PnL may swing with the Mark Price, realised PnL is determined using actual traded price and is unimpacted by Mark Price.