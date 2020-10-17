---
layout: default
title: Turbo Options
has_children: false
parent: Options Guide
nav_order: 2
---

# Turbo Options Guide

1. Table of Contents
{:toc}

## Turbo Options: Motivation & Use Cases
Turbo options are exotic options that combine vanilla call/ put options with a knockout barrier to create a different return/ risk profile. A vanilla option has a fixed expiry time. This is what the Knockout barrier changes. A Turbo options expires instantly (gets 'knocked out') if the price of the option's underlying asset touches/ crosses the Knockout barrier. This risk of pre-mature expiry helps to reduce the price of Turbo options and consequently, increases the inherent leverage in these instruments. 

The Turbo options that we offer on Delta Exchange have another defining characteristic - these options are always deep in the money. An call/ put option is 'in the money' when its strike price is less/ greater than the current price of the underlying assets. The more the money-ness of an options, the more it behaves like a futures. If you are famililar with options greeks, you would know that the delta of a deep in the money option is very close to 1. The upshot of these design choices is that for practical purposes, a position in our Turbo options for behaves like a leveraged long (for Turbo calls) or short (for Turbo puts) position in the underlying asset which: (a) must be closed at the time of expiry and (b) has a fixed stoploss at the Knockout barrier.

The leverage inherent in a Turbo option is directly connected to the relative position of the underlying's price and Knockout Price. The closer the spot price to the Knockout Price, the higher the probability of the option getting 'knocked out' and thus, lower the price of the option. Since, the contract size remains unchanged, as price of the option goes down, the effective leverage it offers goes up. The leverage of Turbo options at the time of launch is 200x. As spot price moves towards/ away from the Knockout Price, the leverages goes up/ down.

![image]({{site.baseurl}}/assets/images/turbo_options.jpeg "Pay-off Profiles of Turbo Options")

## Types of Turbo Options
We currently offer Turbo options only on Bitcoin. These are of two types:

- **Turbo Call Options:** Turbo call options increase in value when price of the underlying goes up. Therefore, a long position in a Turbo Call is akin to a leveraged long position in the underlying asset. You should buy Turbo calls when you are bullish on the underlying asset.

- **Turbo Put Options:** Turbo put options increase in value when price of the underlying goes down. Therefore, a long position in a Turbo Put is akin to a leveraged short position in the underlying asset. You should buy Turbo puts when you are bearish on the underlying asset.

Besides, the expiry date a Turbo option is defined by two price levels:

- **Strike Price:** This is the strike price of the constituent call/ put option of the Turbo option

- **Knockout Price:** This is the price level the breach of which by the underlying's price leads to immediate expiration of the Turbo opion.

Currently, we offer Turbo options only on BTC. These options have a lifespan of 1 day. Everyday, new Turbo options are listed at 11:55am UTC. The trading in these options begin 5 minutes later at 12pm UTC and unless knocked out, are settled the followed day at 12pm UTC.

## Mechanics of Turbo Options

### **Pricing**

Because the Turbo options on Delta Exchange are deep in the money, they can be priced as delta-one instruments. Therefore, the fair value of these options can be written as:

**Turbo Calls**

$$Fair\ Value = Spot\ Price - Knockout\ Price + Spot\ Price * Time-to-expiry * Funding\ Rate$$

**Turbo Puts**

$$Fair\ Value = Knockout\ Price - Spot\ Price  + Spot\ Price * Time-to-expiry * Funding\ Rate$$

where, Funding Rate is the implied cost of carry for a long position. Funding Rate is not directly observable, but can be imputed from the prices that are seen in the order book.

Further note that the smaller the distance between Underlying Index Price and Knockout Price, the lower is the fair value of a Turbo option.



### **Mark Price**

Turbo options are marked at the absolute value of the difference between price of the underlying and the Knockout Price

$$Mark\ Price = absolute (Underlying\ asset\ price - Knockout\ Price)$$

It is worth nothing that for Turbo call options, Underling asset price must always stay above the Knockout Price. This is because a Turbo call option expires instantly when the Underlying asset price goes below the Knockout Price.

Conversely, for Turbo put options, the Underlying asset price must always stay below the Knockout Price, as these options expire when Underlying asset price goes above the Knockout Price.

### **Profit/ Loss Equation**

The Premium for a long Turbo option trade is directly subtracted from the Available Balance. The cashflow that is received when a long position in a Turbo option is closed is referred to as **Pay-off**. The Profit/ Loss of a long position in a Turbo option can thus be computed as 

$$ Premium = Num\_of\_contracts * Contract\ Value * Entry\ Price$$

$$Pay-off = Num\_of\_contracts * Contract\ Value * Mark\ Price$$

$$ Profit/\ Loss = Pay-off - Premium $$

$$Profit/\ Loss = Num\_of\_contracts * Contract\ Value * (Mark\ Price - Entry\ Price)$$

Please do note the following:

- A Turbo option instantly expires when the Underlying asset price crosses the Knockout Price. A knocked out option does not have any residual value.

- Only designated market makers can take net short positions in Turbo options. All other traders can only take long positions. Depending up on whether your view is bullish or bearish, you can buy Turbo call or Turbo put options.


### **Examples**

You buy 1000 contracts of a BTC Turbo Call (Strike Price = 10,000 and Knockout Price = 11250) for USDT 100. At the time, the spot price of BTC is 11345. Further, the Turbo call is set to expire in 6 hours.


**Scenario 1** BTC spot price goes to 11248 after 2 hours

Since the spot price has gone below the Knocout Price, the Turbo Calls expire immediately with a residual value of 0. You realise a loss of USDT 100.

**Scenario 2:** BTC spot price is at 12000 when the Turbo calls expire

At settlement, you get a pay-off of USDT 750. Thus, you realise a profit of USDT (750 - 100) = USDT 650 in the trade.

**Scenario 3:** BTC spot price is at 11300 when the Turbo calls expire

At settlement, you get a pay-off of USDT 50. Thus, you realise a loss of USDT (50 - 100) = - USDT 50 in the trade.


### **Liquidations**

Long options positions can never get liquidated. This holds true for Turbo options as well.

## Trading Fees

Turbo options do not have any trading fees, i.e. both marker and taker fees are 0%. 

The trading fees schedule for all the contracts listed on Delta Exchange is available [here](https://wwww.delta.exchange/fees).


## Expired Turbo options
The Settlement Prices of expired options and futures contracts are available on this [page](https://www.delta.exchange/app/expired_futures).

- 





