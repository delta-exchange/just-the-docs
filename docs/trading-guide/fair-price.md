---
layout: default
title: Fair Price Marking
has_children: false
parent: Trading Guide
nav_order: 1
---

# Fair Price Marking

1. TOC
{:toc}

Leverage that is inherent in derivative contracts, combined with the high volatility of cryptocurrencies, can lead to unwarranted liquidations. Lack of liquidity could further exacerbate this situation as price swings in a derivative contract relative to the underlying index could widen even further.

To avoid such issues and ensure a smooth trading experience, Delta Exchange marks all open position at Fair Mark Price instead of Last Traded Price. The Mark Price has lesser volatility and is more resilient against attempts to manipulate the market.

It is worth noting that Fair Price marking is relevant only for the computation of Unrealized PnL and Liquidation price. Realized PnL is based off actual traded prices and is thus not impacted by Mark Price.

## Impact Price

Before going on to explaing how Mark Price is computed, we first need to introduce the notion of Impact Price. This price tries to estimate the price at which a typical long or short position (called Impact Position) in the futures contract can be entered at any given time.

Impact Size, in terms of number is contracts to be traded, is provided in the [specifications](https://www.delta.exchange/contracts/) for each contract. It is easy to see that Impact Price is a function of: (a) Impact Size and (b) current state of the order book.

$$Impact\ Bid\ Price = Average\ fill\ price\ to\ execute\ a\ typical\ short\ trade$$

$$Impact\ Ask\ Price = Average\ fill\ price\ to\ execute\ a\ typical\ long\ trade$$

$$Impact\ Mid\ Price = Average\ of\ Impact\ Bid\ Price\ and\ Impact\ Ask\ Price$$


## Mark Price of Futures & Perpetual Contracts

The price of a futures contract converges to the underlying index price at the time of contract maturity, i.e.

$$Futures\ Price = Underlying\ Index\ Price$$

At all other times, the price of a futures contract broadly moves in tandem with the price of the Underlying Index, with the difference between the two referred to as basis, i.e.

$$Basis = Futures\ Price - Underlying\ Index\ Price$$

Since the Underlying Index is the foundation of the futures contract, it is logical to assume that

$$Futures\ Fair\ Price = Underlying\ Index\ Price + Fair\ Basis$$

The $$Underlying\_Index\_Price$$ is obviously independent of the trading happening on Delta Exchange and is sourced in real-time from leading spot exchanges.


### Fair Basis Calculation

We first compute an annualised fair value basis, %AnnualisedBasis:

$$\%AnnualisedBasis = (Impact\ Mid\ Price/ Underlying\ Index\ Price - 1) * (365*86400/ time\ to\ expiry\ in\ sec)$$

**Note that For perpetual contracts $$time\_to\_expiry$$ is always 8 hours.**

%AnnualisedBasis is computed only once every 5 seconds. Further, if the market is illiquid, i.e. 

$$(Impact\ Ask\ Price - Impact\ Bid\ Price) > Maintenance\ Margin$$ 

%AnnualisedBasis is not updated. 

Next, %FairBasis is computed as the moving average of the 12 most recent values of %AnnualisedBasis. To avoid anomalous values, the %FairBasis is bounded by certain hard limits, which can vary from contract to contract. 

Fair Basis is computed using %FairBasis as per the following equation:

$$Fair\ Basis = Underlying\ Index\ Price * \%Fair\ Basis * (time\ to\ expiry\ in\ sec/ (365* 86400))$$

Now that we have the Fair\ Basis, the Mark Price of the contract can be easily computed:

$$Futures\ Mark\ Price = Underlying\ Index\ Price + Fair\ Basis$$

It is worth noting that only live positions are marked using the Mark Price. Thus, while unrealised PnL may swing with the Mark Price, realised PnL is unimpacted by Mark Price and depends only on the actual traded prices.


## Mark Price of Calendar Spread Contracts

The mark price of calendar spread contracts is computed as the difference of the fair prices of the two futures contracts that underlie the spread contract, with the fair prices computed as per the following formula:

$$ Fair\ Price = Spot\ Price + moving\ average (annualised\ basis) * (time\ to\ expiry\ in\ sec/ (365* 86400))$$

Therefore, the mark price of a calendar spread contract can be written as:

$$ Mark\ Price = Fair\ Price\ longer\ dated\ futures - Fair\ Price\ shorter\ dated\ futures$$

## Mark Rate of BitMex Funding Rate Swap

Open positions in the BitMex Funding Rate swap product are marked at the annualised implied rate from the basis of the BTC futures of the same maturity as the swap. BitMex published fair basis of the relevant BTC futures. We use the annual rate implied by this fair basis as the mark rate.

The basis implied annual rate tends to become quite volatile as the futures contract heads into expiry. To keep the mark rate stable and avoid unnecessary liquidations, when the maturity of the swap contract is less tha 10 days away, we start using the basis implied annual rate of the next quarterly BTC futures.   

## Fair Price Marking Leading into Settlement

Contracts on Delta Exchange are settled on the 30 minute TWAP (time-weighted average price) of its Underlying Index. To ensure that at the time of settlement there are no under-margined positions, the transition from using Underlying Index Price to TWAP of the Underlying Index Price in the calculation of Mark Price is done gradually. 

When the settlement time of a contract is one hour away, we switch to a weighted average of Underlying Index Price and TWAP of the Underlying Index Price in Mark Price computation. At the time of switch, weight of Underlying\_Index\_Price$$ is $$100%$$ and weight of $$TWAP\_Underlying\_Index\_Price$$ is $$0%$$. Over the next $$30$$ minutes, these weights are changed every minute such that weights converge to 0% and 100% respectively. This means that in the last 30 minutes leading into contract settlement, it is $$TWAP\_Underlying\_Index\_Price$$ that is used in the computation of the Mark Price.