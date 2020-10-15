---
layout: default
title: Profit Loss Math
has_children: false
parent: Trading Guide
nav_order: 5
---

# Profit Loss Math

All open positions on Delta are marked at the [fair price]({{site.baseurl}}/docs/trading-guide/fair-price/#fair-price-marking) of the Futures contract. Thus, Unrealized PnL and Liquidation Prices are computed using Fair Prices, while Realized PnL is based on actual entry and exit prices.

PnL for a long/ short position in a Vanilla Futures

$$PnL = ± n*m*(Fut\_CurrentPrice - Fut\_EntryPrice)$$

PnL for a long/ short position in an Inverse Futures

$$PnL = ± n*m*(1/ Fut\_EntryPrice - 1/ Fut\_CurrentPrice)$$

 where $$m$$ is the multiplier and $$n$$ is the position size (i.e. number of contracts).

If a position is acquired at multiple entry prices, an average entry price is computed and used for PnL computation.

