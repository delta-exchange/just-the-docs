---
layout: default
title: Allowed Trading Bands
has_children: false
parent: Margin Trading Guide
nav_order: 9
---

# Allowed Trading Band

1. Table of Contents
{:toc}

To prevent market manipulation as well as idiosyncratic price moves trading on Delta Exchange can take place only within the allowed trading band. Generally, speaking the allowed trading band is defined around the prevailing [Mark Price]({{site.baseurl}}/docs/trading-guide/fair-price/#fair-price-marking). However, the exact methodology for creating the trading band differs from different product types. 

Please do note that trading bands are not applicable to liquidation orders.

## Futures & Perpetual Contracts

**Band1: Price Volatility Band**

Band1 is defined as a +/-2 standard deviation band around the current Mark Price. The standard deviation of the Mark Price in the last 15 minutes. 

$$UpperBand1=Mark\ Price + 2 * Standard\ Deviation (Mark\ Price)$$

$$LowerBand1 = Mark\ Price - 2 * Standard\ Deviation (Mark\ Price)$$

Thus, Band1 expands when market is volatile and contracts when there's little volatility in the market.


**Band2: Price Range Band**

Band2 is defined around the current Mark Price in terms of percentage of Mark Price. 

$$UpperBand2=Mark\ Price + Mark\ Price (1 + Range/ 100)$$

$$LowerBand2 = Mark\ Price - Mark\ Price (1 - Range/ 100)$$

where value of Range is provided as Price Band in a contract's specification.

The allowed trading band for futures and perpetual contracts is computed by combining Band1 and Band2 in such a way that on either side the wider of the two bands is selected.

$$UpperBand = max (UpperBand1, UpperBand2)$$

$$LowerBand = min (LowerBand1, LowerBand2)$$

## Calendar Spread Contracts

The allowed trading band for calendar spread contracts is created by combining two different bands which are described below:

**Band1: Price Volatility Band**

Band1 is defined as a +/-2 standard deviation band around the current Mark Price. The standard deviation of the Mark Price in the last 15 minutes. 

$$UpperBand1=Mark\ Price + 2 * Standard\ Deviation (Mark\ Price)$$

$$LowerBand1 = Mark\ Price - 2 * Standard\ Deviation (Mark\ Price)$$

**Band2: Price Range Band**

Band2 is defined around the current Mark Price in terms of percentage of Spot Price. 

$$UpperBand2=Mark\ Price + Spot\ Price * Range/100$$

$$LowerBand2 = Mark\ Price - Spot\ Price * Range/100$$

where value of Range is provided as Price Band in a contract's specification.

Band1 and Band2 are combined in such a way that on either side the wider of the two bands is selected.

$$UpperBand = max (UpperBand1, UpperBand2)$$

$$LowerBand = min (LowerBand1, LowerBand2)$$


## Options

The allowed trading band for options (call, put and MOVE) contracts is created by combining three different bands which are described below:

**Band1: Price Volatility Band**

Band1 is defined as a +/-2 standard deviation band around the current Mark Price. The standard deviation of the Mark Price in the last 15 minutes. 

$$UpperBand1=Mark\ Price + 2 * Standard\ Deviation (Mark\ Price)$$

$$LowerBand1 = Mark\ Price - 2 * Standard\ Deviation (Mark\ Price)$$

**Band2: Implied Volatility Band**

This band is created by computing the theoretical prices of the options contract at mid implied volatility +/- IV Range. The mid implied volatility is average of implied volatility for impact bids and impact offers.

$$UpperBand2 = Black\ Scholes\ Price (Mid\ Implied\ Volatility + IV\_Range, Spot\ Price, Time\ to\ Expiry)$$


$$LowerBand2 = Black\ Scholes\ Price (Mid\ Implied\ Volatility - IV\_Range, Spot\ Price, Time\ to\ Expiry)$$

**Band3: Price Range Band**

Band3 is defined around the current Mark Price in terms of percentage of Spot Price. 

$$UpperBand3=Mark\ Price + Spot\ Price * Range/100$$

$$LowerBand3 = Mark\ Price - Spot\ Price * Range/100$$

where value of Range is provided as Price Band in a contract's specification.

The allowed trading band for options is computed by combining Band1, Band2 and Band3 in such a way that on either side the wider of the two bands is selected.

$$UpperBand = max (UpperBand1, UpperBand2, UpperBand3)$$

$$LowerBand = min (LowerBand1, LowerBand2, LowerBand3)$$

## Interest Rate Swaps

The allowed trading band for interest rate swap (IRS) contracts is created by combining three different bands which are described below:

**Band1: Rate Volatility Band**

Band1 is defined as a +/-2 standard deviation band around the current Mark Rate. The standard deviation of the Mark Rate in the last 15 minutes. 

$$UpperBand1=Mark\ Rate + 2 * Standard\ Deviation (Mark\ Rate)$$

$$LowerBand1 = Mark\ Rate - 2 * Standard\ Deviations (Mark\ Rate)$$

**Band2: Rate Range Band**

Band3 is defined around the current Mark Rate in terms of percentage of maximum (Vmax) and minimum (Vmin) values that the interest rate over which the swap contract is defined is allowed to take.

$$UpperBand2=Mark\ Price + max (Vmax, abs(Vmin)) * Range/100$$

$$LowerBand2=Mark\ Price - max (Vmax, abs(Vmin)) * Range/100$$

where value of Range is provided as Price Band in a contract's specification.

**Band3: Rate Limit Band**

Since the interest rate over which the IRS contract is defined is bounded between (Vmin, Vmax), no trading should happen outside this band. Therefore,

$$UpperBand3 = Vmax $$

$$LowerBand3=Vmin$$

Note that Vmax and Vmin are part of the contract's specification

The allowed trading band for IRS contracts is derived by combining Band1, Band2 and Band3 as per the following equation:

$$UpperBand = min (max (UpperBand1, UpperBand2), UpperBand3)$$

$$LowerBand = max (min (LowerBand1, LowerBand2), LowerBand3)$$


## Impact of the Trading Band on various types of orders

- **[Market orders]({{site.baseurl}}/docs/trading-guide/OrderTypes/#market-orders):** Any part of your market order that will breach the trading band on execution will be converted into a limit order with price equal to the max (for buy orders) or min (for sell orders) allowed price.
- **[Limit orders]({{site.baseurl}}/docs/trading-guide/OrderTypes/#limit-orders):** If the price of your buy limit order exceeds the Max Price, the price of the order will be automatically set to the max buy price. Conversely, if the price of your sell limit order is below the Min Price, the price of the order will be automatically set to the min sell price.
- **[Stop market orders]({{site.baseurl}}/docs/trading-guide/OrderTypes/#stop-orders):** Once triggered, a stop market order will behave in the exact same manner as a market order. This means that part or all of the order could be converted into a limit order if its execution will breach the trading band in force at the time the stop market order is triggered.
- **[Stop limit orders]({{site.baseurl}}/docs/trading-guide/OrderTypes/#stop-orders):** Once triggered, a stop limit order will behave in the exact same manner as a limit order. This means that the limit price of the order may be changed in accordance with the trading band in force at the time the stop limit order is triggered.
- **[Bracket orders]({{site.baseurl}}/docs/trading-guide/OrderTypes/#bracket-orders):** Bracket orders are akin to two stop market orders (a take-profit order and a stoploss order) wherein triggering of one order cancels the another. Once one of the orders in a bracket order is triggered, it behaves in the exact same manner as a stop market order.

## Changing behaviour of market orders using order validity attributes
[Order validity attributes]({{site.baseurl}}/docs/trading-guide/OrderTypes/#order-validity-attributes) are also known as time in force flags. The default time in force flag for market orders as well as stop market orders is GTC (Good till cancelled). Traders using the website do not have the option to changing this flag. However, if you are trading using APIs, then you have the option to place IOC (immediate or cancel) market order and stop market orders. When you use an IOC flag, any part of the market (or stop market) order that will breach the trading band will be cancelled.