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
Turbo options are exotic options that combine vanilla call/ put options with a knockout barrier to create a different return/ risk profile. A vanilla option has a fixed expiry time. This is what the Knockout barrier changes. A Turbo options immediately expires if the price of the option's underlying asset touches/ crosses the Knockout barrier. This risk of pre-mature expiry helps to reduce the price of Turbo options and consequently, increases the inherent leverage in these instruments. 

The Turbo options that we offer on Delta Exchange have another defining characteristic - these options are always deep in the money. An call/ put option is 'in the money' when its strike price is less/ greater than the current price of the underlying assets. The more the money-ness of an options, the more it behaves like a futures. This means a position in our Turbo options for practical purposes behaves like a leveraged position in the underlying assets which: (a) must be closed at the time of expiry and (b) has a fixed stoploss at the Knockout barrier.

The leverage inherent in a turbo option is directly connected to the relative position of the underlying's price and Knockout Price. The closer the spot price to the Knockout Price, the higher the probability of the option getting 'knocked out' and thus, lower the price of the option. Since, the contract size remains unchanged, as price of the option goes down, the effective leverage it offers goes up. The leverage of Turbo options at the time of launch is 200x. As spot price moves towards/ away from the Knockout Price, the leverages goes up/ down.


## Types of Turbo Options
We currently offer Turbo options only on Bitcoin. These are of two types:

- **Turbo Call Options:** Turbo call options increase in value when price of the underlying goes up. You should buy Turbo calls when you are bullish on the underlying asset. 

- **Turbo Put Options:** Turbo put options increase in value when price of the underlying goes down. You should buy Turbo puts when you are bearish on the underlying asset.

## Mechanics of Turbo Options

### Pricing

Because the Turbo options on Delta Exchange are deep in the money, they can be priced as delta-one instruments. Therefore, the fair value of these options can be written as:

**Turbo Calls**

$$Fair\ Value = Underlying\ Index\ Price - Knockout\ Price + Underlying\ Index\ Price * Time\_to\-expiry * Funding\ Rate$$

**Turbo Puts**

$$Fair\ Value = Knockout\ Price - Underlying\ Index\ Price  + Underlying\ Index\ Price * Time\_to\-expiry * Funding\ Rate$$

where, Funding Rate is the implied cost of carry for a long position. Funding Rate is not directly observable, but can be imputed from the prices seen in the order book.

Further note that the smaller the distance between Underlying Index Price and Knockout Price, the lower is the fair value of a Turbo option.



### Mark Price

Turbo options are marked at the absolute value of the difference between price of the underlying and the Knockout Price

$$Mark Price = absolute (Underlying\ asset\ price - Knockout\ Price)$$

It is worth nothing that for Turbo call options, Underling asset price must always stay above the Knockout Price. This is because a Turbo call option expires instantly when the Underlying asset price goes below the Knockout Price.

Conversely, for Turbo put options, the Underlying asset price must always stay below the Knockout Price, as these options expire when Underlying asset price goes above the Knockout Price.

### Profit/ Loss Equation

The Premium for a long Turbo option trade is directly subtracted from the Available Balance. The cashflow that is received when a long position in a Turbo option is closed is referred to as **Pay-off**. The Profit/ Loss of a long position in a Turbo option can thus be computed as 

$$ Premium = Num\_of\_contracts * Contract\ Value * Entry\ Price$$

$$Pay-off = Num\_of\_contracts * Contract\ Value * Mark\ Price$$

$$ Profit/\ Loss = Pay-off - Premium $$

$$Profit/\ Loss = Num\_of\_contracts * Contract\ Value * (Mark\ Price - Entry\ Price)$$

Please do note the following:

- A Turbo option instantly expires when the Underlying asset price crosses the Knockout Price. A knocked out option does not have any residual value.

- Only designated market makers can take net short positions in Turbo options. All other traders can only take long positions. Depending up on whether your view is bullish or bearish, you can buy Turbo call or Turbo put options.

### Liquidations

Long options positions can never get liquidated. This holds true for Turbo options as well.

## Trading Fees

Turbo options do not have any trading fees, i.e. both marker and taker fees are 0%. 

The trading fees schedule for all the contracts listed on Delta Exchange is available [here](https://wwww.delta.exchange/fees).


## Expired Turbo options
The Settlement Prices of expired options and futures contracts are available on this [page](https://www.delta.exchange/app/expired_futures).

- 





