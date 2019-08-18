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
- The price of a futures contract and its underlying can be quite different, with these two prices being guaranteed to converge at the contract expiry. Perpetual contracts by design trade close to the price of the underlying (spot). The closeness of perpetual price and spot price is acheived through funding which is explained below.
- The above property makes trading perpetual contracts akin to trading spot markets on leverage, i.e. margin trading.

**Benefits of a perpetual contracts vs. futures**

- Since a futures contract has an expiry date, a trader looking to maintain his position will need to periodically roll to a next contract as the previous one expires. Perpetual contracts obviate the need to roll positions.
- The difference between price of a futures and its underlying (i.e. the basis) can vary quite a bit. This exposes futures traders to basis risk. Since perpetual contract alaways trades close to spot market, the basis risk is minimal and bounded.

## Funding Rate Explanation

Funding is the primary mechanism which tethers price of a perpetual contract to spot. Funding is a series of continuous payments that are exchanged between longs and shorts in a perpetual contract. Let's understand how funding helps keep price of the perpetual contract close to the spot price.

**Perpetual contract price > Spot price**

When a perpetual contract trades at a **premium** to spot, funding is **positive**, i.e longs pay funding to shorts. This creates disincentive to stay long or enter into a new long position. Conversely, it creates incentive to stay short or enter into a new short position. These dynamics will serve to push the price of the perpetual contract down towards the spot price.

**Perpetual contract price < Spot price**

When a perpetual contract trades at a **discount** to spot, funding is **negative**, i.e. shorts pay funding to longs. This creates disincentive to stay short or enter into a new short position. Conversely, it creates incentive to stay long or enter into a new long position. These dynamics will serve to push the price of the perpetual contract up towards the spot price.

## Funding Rate Calculation

**Premium Rate**

$$Premium Rate = (Mark Price - Underlying\_Index\_Price)/ Underlying\_Index\_Price$$

The details on how the Mark Price is calculated are available [here](https://www.delta.exchange/user-guide/docs/trading-guide/fair-price/)

**Funding Rate**

$$Funding\ Rate = Premium\ Rate$$

Funding Rate is considered to be an 8-hourly interest rate and is computed every minute. The magnitude of Funding Rate is capped. Funding caps could vary from contract to contract and are availabe in the [contract specifications](https://www.delta.exchange/contracts/)

**Funding Payment** 

Funding is exchanged between longs and shorts every minute. It is impportant to note that while accruals for funding paid or received happen every minute, entries for funding payments are made in the transaction log once every 2 hours or when a position is closed. Funding payments are completely peer-to-peer and Delta Exchange does not charge any fees on funding. Funding paid or received is computed as:

$$Funding\ Payment = Current\_Position\_Value * Funding\ Rate * 1/ (8 * 60)$$

where Current Position Value is value of a the position at the current Underlying Index Price.

**Funding Limits**

A perpetualc contract can be thought as an 8-hour futures contract that is being rolled into the next 8-hour futures every minute. Thus, at any time, the [fair basis]({{site.baseurl}}/docs/trading-guide/fair-price/#fair-basis-calculation) of a perpetual contract should be similar to a futures contract. With this in mind, we cap the fair basis for perpetual swap at -/+0.5%. This limit on fair basis results in the same constraint being applied to the Funding Rate. Consequently, the Funding rate must always stay between -0.45% and 0.45%.

**Funding Example**

Lets say you have a long position of 10000 contracts in the BTCUSD Perpetual contract on Delta Exchange. Recall that 1 BTCUSD contract is 1 USD.

Between time $$T$$ and $$T+30\ mins$$, $$Mark Price = $4015,$$ and $$Underlying\ Index\ Price = $4000$$

$$Premium\ Rate = (4015-4000)/4000 = 0.375\%$$

$$Funding\ Rate = Max (0.05\%,0.375\%) + Min (-0.05\%,0.375\%)$$

$$= 0.375\% - 0.05\% = 0.325\%$$

Since you are long and Funding Rate is positive, you'd be paying funding.               

$$ Funding\ Paid\ per\ min = 10000 * (1/ 4000) * 0.325\% * 1/ (8*60) =  0.00001693  BTC$$

Since both Underlying Index Price and Mark Price remain unchanged for this 30 min window, the funding paid by you in 30 mins can be computed as:

$$ Funding\ Paid\ for\ 30\ mins = 30 * 0.00001693 = 0.000508 BTC$$

## Funding for OTC Contracts

For OTC contracts, funding rate is not computed using the order book. Instead, funding rate is charged by the party providing liquidity to the party that demands liquidity. While the funding rate generally stays constant, it can change with the lqiudity situation in the market or with sharp price moves. All other dynamics of funding remain unchanged, i.e. in OTC contracts too: (a) funding is peer to peer, (b) if funding rate is positive: longs pay shorts and if funding rate is negative: shorts pay longs, and (c) funding is exchanged between longs and shorts every minute.





