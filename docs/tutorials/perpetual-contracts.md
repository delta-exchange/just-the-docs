---
layout: default
title: Perpetual Contracts Guide
nav_order: 3
---

# Perpetual Contracts Guide

1. Table of Contents
{:toc}

## Perpetual Contracts: Motivation & Use Cases

Perpetual contracts are a type of derivatives that are similar to a [futures contract](https://www.delta.exchange/user-guide/docs/tutorials/futures-guide/), but with some key differentiating properties:

- Unlike futures, perpetual contracts do not have an expiry date
- The price of a futures contract and its underlying can be quite different, with these two prices being guaranteed to converge at the contract expiry. Perpetual contracts by design trade close to the price of the underlying (spot). The closeness of perpetual price and spot price is achieved through funding which is explained below.
- The above property makes trading perpetual contracts akin to trading spot markets on leverage, i.e. margin trading.

**Benefits of a perpetual contracts vs. futures**

- Since a futures contract has an expiry date, a trader looking to maintain his position will need to periodically roll to a next contract as the previous one expires. Perpetual contracts obviate the need to roll positions.
- The difference between price of a futures and its underlying (i.e. the basis) can vary quite a bit. This exposes futures traders to basis risk. Since perpetual contract always trades close to spot market, the basis risk is minimal and bounded.

## Funding Rate Explanation

Funding is the primary mechanism which tethers price of a perpetual contract to spot. Funding is a series of continuous payments that are exchanged between longs and shorts in a perpetual contract. Let's understand how funding helps keep price of the perpetual contract close to the spot price.

**Perpetual contract price > Spot price**

When a perpetual contract trades at a **premium** to spot, funding tends to be **positive**, i.e. longs pay funding to shorts. This creates disincentive to stay long or enter into a new long position. Conversely, it creates incentive to stay short or enter into a new short position. These dynamics will serve to push the price of the perpetual contract down towards the spot price.

**Perpetual contract price < Spot price**

When a perpetual contract trades at a **discount** to spot, funding tends to be **negative**, i.e. shorts pay funding to longs. This creates disincentive to stay short or enter into a new short position. Conversely, it creates incentive to stay long or enter into a new long position. These dynamics will serve to push the price of the perpetual contract up towards the spot price.

## Funding Rate Calculation

Funding Rate is considered to be an 8-hourly rate and is the sum of two terms: (a) Premium and (b) Interest Rate.

**Premium**

$$Premium = (Mark\  Price - Underlying\_Index\_Price)/ Underlying\_Index\_Price$$

The details on how the Mark Price is calculated are available [here](https://www.delta.exchange/user-guide/docs/trading-guide/fair-price/). 

Premium is measured every minute and its 8-hour TWAP (Avg Premium) is used in the computation Funding Rate.

**Interest Rate**

The Interest Rate term in Funding calculation is a function of the differential of borrow rates of quote currency and base currency of the perpetual contract. The Interest Rate thus is a proxy for cost of holding a position in a perpetual contract. 

Since borrow rates for different currencies can vary widely, the Interest Rate used in Funding calculations can vary from contract to contract. However, as of now, Interest Rate of 0.01%/ 8 hours is being used for contracts. 

**Funding Rate**

Funding Rate is computed using the following formula:

$$Funding\ Rate = Avg\ Premium + clamp (Interest\ Rate - Avg\ Premium, 0.05\%, -0.05\%)$$

The clamp function limits the value of (Interest Rate - Avg Premium) to a band of (-0.05%, 0.05%). 

This means that if (Interest Rate - Avg Premium) is within +/-0.05%, Funding Rate is equal to:

$$Funding\ Rate = Avg\ Premium + (Interest\ Rate - Avg\ Premium) = Interest\ Rate$$

When (Interest Rate - Avg Premium) < -0.05%, then Funding Rate is equal to:

$$Funding\ Rate = Avg\ Premium - 0.05\%$$

And, when (Interest Rate - Avg Premium) > 0.05%, then Funding Rate is equal to:

$$Funding\ Rate = Avg\ Premium + 0.05\%$$

Funding Rate is computed 3 times in a 24 hour period at: 8am UTC, 4pm UTC and 12am UTC. At these times, the TWAP of Premium in the preceding 8 hours is used to compute the Funding Rate. This Funding Rate thus obtained remains applicable for the next 8 hours. 

At any instant, there are two Funding Rates available: (a) the Funding Rate that is currently applicable and (b) the estimate of the Funding Rate that will be applicable in the next 8 hour interval. Both these rates are available on the price ticker on the top of the trading terminal.

![image]({{site.baseurl}}/assets/images/funding_ticker.jpg "Current and Estimated Next Funding Rates")


**Funding Payment** 

Funding is exchanged between longs and shorts every minute. It is important to note that while accruals for funding paid or received happen every minute, entries for funding payments are made in the transaction log once every 2 hours or when a position is closed. Funding payments are completely peer-to-peer and Delta Exchange does not charge any fees on funding. Funding paid or received is computed as:

$$Funding\ Payment = Current\_Position\_Value * Funding\ Rate * 1/ (8 * 60)$$

where Current Position Value is value of a the position at the current Underlying Index Price.

**Funding Limits**

A perpetual contract can be thought as an 8-hour futures contract that is being rolled into the next 8-hour futures every minute. Thus, at any time, the [fair basis]({{site.baseurl}}/docs/trading-guide/fair-price/#fair-basis-calculation) of a perpetual contract should be similar to a futures contract which will expire in 8 hours. With this in mind, we enforce pretty tight caps on the fair basis for perpetual contracts. As of now, most perpetual contracts have funding capped at 0.5% or 0.15% (for alt-btc pairs). But these caps are subject to change and are available in the [contract specifications](https://www.delta.exchange/contracts/).

**Funding Example**

Lets say you have a long position of 10000 contracts in the BTCUSD Perpetual contract on Delta Exchange. Recall that 1 BTCUSD contract is 1 USD.

Between 8am UTC and 4pm UTC, the TWAP of Premium was 0.04%. Assuming Interest Rate is 0.01%, for the next 8 hours, i.e. between 4pm UTC and 12am UTC, the applicable Funding Rate will be:

$$Funding\ Rate = 0.04\% + clamp(0.01\% - 0.04\%, 0.05\%, -0.05\%) = 0.01\%$$

Since you are long and Funding Rate is positive, you'd be paying funding.      

Let's assume that you hold this long position from 5pm UTC through to 5:30pm UTC. During this time, both the Mark Price and Underlying Index Price stay flat at $4015 and $4000 respectively.         

$$ Funding\ Paid\ per\ min = 10000 * (1/ 4000) * 0.01\% * 1/ (8*60) =  0.00000052  BTC$$

And, the funding paid by you in 30 mins can be computed as:

$$ Funding\ Paid\ for\ 30\ mins = 30 * 0.00001693 = 0.00001562 BTC$$

## Funding for OTC Contracts

For OTC contracts, funding rate is not computed using the order book. Instead, funding rate is charged by the party providing liquidity to the party that demands liquidity. While the funding rate generally stays constant, it can change with the liquidity situation in the market or with sharp price moves. All other dynamics of funding remain unchanged, i.e. in OTC contracts too: (a) funding is peer to peer, (b) if funding rate is positive: longs pay shorts and if funding rate is negative: shorts pay longs, and (c) funding is exchanged between longs and shorts every minute.





