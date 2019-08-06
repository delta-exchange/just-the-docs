---
layout: default
title: Allowed Trading Bands
has_children: false
parent: Margin Trading Guide
nav_order: 8
---

# Allowed Trading Band
To prevent market manipulation as well as idiosyncratic price moves trading on Delta Exchange can take place only within the allowed trading band. This band is defined as 2.5% range around the current [Mark Price]({{site.baseurl}}/docs/trading-guide/fair-price/#fair-price-marking) and is applicable depending up on the direction of a trade. For buy trades, the uper bound (Max Price) of the trading band is applicable. This means any buy order cannot execute at a price greater than Mark Price + 2.5%. Similarly, for sell orders, the lower bound (Min Price) of the trading band is applicable. This implies that the execution price of a sell order cannot be less than Mark Price - 2.5%.


## Impact of the Trading Band on various types of orders

- **Market orders:** Any part of your market order that will breach the trading band on execution will be converted into a limit order with price equal to the max (for buy orders) or min (for sell orders) allowed price.
- **Limit orders:**If the price of your buy limit order exceeds the Max Price, i.e. Mark Price + 2.5%, the price of the order will be automatically set to the max buy price. Conversely, if the price of your sell limit order is below the Min Price, i.e. Mark Price - 2.5%, the price of the order will be automatically set to the min sell price.
- **Stop market orders:**Once triggered, a stop market order will behave in the exact same manner as a market order. This means that part or all of the order could be converted into a limit order if its execution will breach the trading band in force at the time the stop market order is triggered.
- **Stop limit orders:** Once triggered, a stop limit order will behave in the exact same manner as a limit order. This means that the limit price of the order may be changed in accordance with the trading band in force at the time the stop limit order is triggered.
- **Bracket orders:**Bracket orders are akin to two stop market orders (a take-profit order and a stoploss order) wherein triggering of one order cancels the another. Once one of the orders in a bracket order is triggered, it behaves in the exact same manner as a stop market order.

## Changing behaviour of market orders using time in force flags
The default time in force flag for market orders as well as stop market orders is GTC (Good till cancelled). Traders using the website do not have the option to changing this flag. However, if you are trading using APIs, then you have the option to place IOC (immediate or cancel) market order and stop market orders. When you use an IOC flag, any part of the market (or stop market) order that will breach the trading band will be cancelled.