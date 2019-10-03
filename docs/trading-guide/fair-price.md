---
layout: default
title: Fair Price Marking
has_children: false
parent: Margin Trading Guide
nav_order: 1
---

# Fair Price Marking

1. TOC
{:toc}

Leverage is inherent in derivative contracts, combined with high volatility of crypto-currencies can lead to unwarranted liquidations. Lack of liquidity could further exacerbate this situation as price swings in a derivatives contract relative to the underlying index could widen even further.

To ensure a great trader experience, Delta Exchange marks all open position at Fair Price instead of Last Traded Price. The Fair Price has lesser volatility and is more resilient against attempts to manipulate market.

It is worth noting that Fair Price marking is relevant only computation of Unrealized PnL and Liquidation price. Realized PnL is computed using actual trading prices and is thus not impacted by the Fair Price.

## Calculation of Fair Price of a Futures Contract

The price of a futures contract converges to the underlying index price at the time of contract maturity, i.e.

$$Futures\_Price = Underlying\_Index\_Price$$

At all other times, the price of a futures contract broadly moves in tandem with the price of the Underlying Index, with the difference between the two referred to as basis, i.e.

$$Basis = Futures\_Price - Underlying\_Index\_Price$$

Since the Underlying Index is the foundation of the futures contract, it is logical to assume that

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

The $$Underlying\_Index\_Price$$ is obviously independent of the trading happening on Delta Exchange and is sourced in real-time from leading spot exchanges.

## Impact Price

To understand the computation of $$Fair Basis$$, we first need to introduce the notion of $$Impact Price$$. This price tries to estimate the price at which a typical long or short position (called Impact Position) in the futures contract can be entered at any given time.

Impact Size, in terms of number is contracts to be traded, is provided in the [specifications](https://www.delta.exchange/contracts/) for each futures contract. It is easy to see that Impact Price is a function of: (a) Impact Size and (b) current state of the order book.

$$Impact\_Bid\_Price = Average\ fill\ price\ to\ execute\ a\ typical\ short\ trade$$

$$Impact\_Ask\_Price = Average\ fill\ price\ to\ execute\ a\ typical\ long\ trade$$

$$Impact\_Mid\_Price = Average\ of\ Impact\_Bid\_Price\ and\ Impact\_Ask\_Price$$
 

## Fair Basis Calculation

We first compute an annualised fair value basis, $$\%Annualised\_Basis$$:

$$\%Annualised\_Basis = (Impact\_Mid\_Price/ Underlying\_Index\_Price - 1) * (365*86400/ time\_to\_expiry\_in\_sec)$$

**Note that For perpetual contracts $$time\_to\_expiry$$ is always 8 hours.**

$$\%Annualised\_Basis$$ is computed only once 5 seconds. Further, in case at any time of update, market is illiquid, i.e. 

$$(Impact\_Ask\_Price - Impact\_Bid\_Price) > Maintenance\_Margin$$ 

$$\%Annualised\_Basis$$ is not updated. 

Next, $$\%Fair\_Basis$$ is computed as the moving average of the 12 most recent values of $$\%Annualised\_Basis$$. To avoid anomalous values, the $$\%Fair\_Basis$$ is bounded by certain hard limits, which can vary from contract to contract. 

$$Fair Basis$$ is computed using $$\%Fair\_Basis$$ 

$$Fair\_Basis = Underlying\_Index\_Price * \%Fair\_Basis * (time\_to\_expiry\_in\_sec/ (365* 86400))$$

Now, that we have the $$Fair\_Basis$$, the fair price of the futures (i.e. the mark price) can be easily computed:

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

It is worth noting that only live positions are marked using the Fair Price. Thus, unrealised PnL may swing with the Mark Price, realised PnL is determined using actual traded price and is unimpacted by Mark Price.

## Fair Price Marking Leading into Settlement

A Futures contract on Delta Exchange are settled on the $$30$$ minute TWAP (time-weighted average price) of its Underlying Index. To ensure that at the time of settlement there are no under-margined positions, the transition from using $$Underlying\_Index\_Price$$ to $$TWAP\_Underlying\_Index\_Price$$ in the calculation of Mark Price is done gradually. 

When the settlement time of a contract is one hour away, we switch to a weighted average of $$Underlying\_Index\_Price$$ and $$TWAP\_Underlying\_Index\_Price$$ in Mark Price computation. At the time of switch, weight of $$Underlying\_Index\_Price$$ is $$100%$$ and weight of $$TWAP\_Underlying\_Index\_Price$$ is $$0%$$. Over the next $$30$$ minutes, these weights are changed every minute such that weights converge to 0% and 100% respectively. This means that in the last 30 minutes leading into contract settlement, it is $$TWAP\_Underlying\_Index\_Price$$ that is used in the computation of the Mark Price. At contract settlement, $$Fair Basis$$ is $$0$$ and Mark Price converges to $$TWAP\_Underlying\_Index\_Price$$.