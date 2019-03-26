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

$$Funding\ Rate = Max(0.05\%, Premium\ Rate) + Min(-0.05\%, Premium\ Rate)$$

The above equation results in the following outputs:
- when Premium Rate lies between -0.05% and 0.05%, Funding Rate = 0%
- when Premium Rate is greater than 0.05%, Funding Rate = Premium Rate - 0.05%
- when Premium Rate is less than -0.05%, Funding Rate = Premium Rate + 0.05%

Funding Rate is considered to be an 8-hourly interest rate and is computed every minute. 

**Funding Payment** 

Funding is exchanged between longs and shorts every minute. Funding payments are completely peer-to-peer and Delta Exchange does not charge any fees on funding. Funding paid or received is computed as:

$$Funding\ Payment = Current\_Position\_Value * Funding\ Rate * 1/ (8 * 60)$$



