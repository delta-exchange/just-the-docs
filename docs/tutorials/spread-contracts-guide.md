---
layout: default
title: Spread Contracts Guide
nav_order: 4
---

# Spread Contracts Guide

1. Table of Contents
{:toc}

## Spread Contracts: Motivation & Use Cases

Spread contracts are a special class of derivative where the underlying is a spread. The spread here could be the difference in prices of any two assets or even derivative contracts. Thus, a position in a spread contract is equivalent to offsetting (long and short respectively) positions in the assets/ contracts that form the spread. 

**Calendar spread contracts:** These spread contracts are defined over the price difference of two futures contracts having the same underlying, but different maturities. To understand this better, let's consider Bitcoin futures spread contract. This contract is defined as the difference in price of a longer-dated BTC futures (e.g. BTC December futures) and the price of a shorter-dated BTC futures (e.g. BTC September futures). Thefore, a position in the BTC futures spread contract represents a long and short position in the two underlying futures contracts.

_**Long** BTC spread futures contract = **Long** longer-dated BTC futures + **Short** shorter-dated BTC futures_


_**Short** BTC spread futures contract = **Short** longer-dated BTC futures + **Long** shorter-dated BTC futures_

It is evident from the above equations, calendar spread contracts should not be used for directional trades on the underlying asset (BTC in this case). Instead, these contracts should be used to experess views on the relative pricing of the constituent futures.

### **Characteristics of calendar spread contracts**

**Market neutral:** Because calendar spread contracts represent offseting positions in two futures contracts of the same underlying, by definition, price of the spread contract should remain unimpacted by the change in price of the underlying. Let us go back to our Bitcoin futures spread contract example. If BTC prices goes up by 5%, the profits from the long position in one of the underlying BTC futures will be offset by short position in the other BTC futures.

This also means that a trade in a calendar spread contract is actually a call on the relative pricing of the two underlying futures. If the longer-dated futures is trading richer compared to the shorter-dated futures, you should short the spread contract. Alternatively, if the longer-dated futures is trading cheaper compared to the shorter-dated futures, you should long the spread contract.

**Lower risk:** Lower risk or price volatility of calendar spread contracts is a natural corollary of their market neutral nature. Moreover, spreads tend to be mean reverting. Lower volatilit allows us to offer higher leverage on spread contracts. This means you can take bigger positions with smaller margin.

**Cost efficient:** For traders looking to gain from mispricing in futures with the same underlying with different expiry dates, taking a position in a spread contrat is a lot more cost efficient than taking long/ short position in the two futures contracts. This is because: 

(a) you need not provide margin for the long and short positions separately. Moreover, leverage allowed in the futures contracts tend to be lower than leverage for spread contracts. Therefore, an equivalent position in spread contracts require lesser margin, and 

(b) you end up paying lower fees because you need to execute just one trade, as opposed to two, when you take a position in a spread contract.


## Mechanics of Calendar Spread Contracts

### **Quotation & Settlement Currencies**

Our BTC futures spread contracts are defined over the differences in prices of our BTC quarterly futures. These futures are inverse contracts, i.e. they are quoted in USD, but their margining and settlement happens in BTC. 

Despite the constituent futures contract being inverse, the spread contracts are vanilla or linear. This enables quoting of USD value of spread for a selected bitcoin notional and thus, makes the mechanics of the contract more intuitive for traders. 

Another detail worth noting here is that even though our BTC futures calendar spread contracts are quoted in USD, the margining currency is USDT. The USDT/ USD conversion rate is built into the contract and has been set at 1.


### **Orderbook**

The order-book on calendar spread contracts on Delta represent the price at which traders are willing to buy or sell the difference between futures on two different maturities.

For the BTC futures calendar spread contract, the spread between the prices of the constituent futures is denominated in USD. Therefore, the price of the spread contract is quoted in USD.

### **Margining for calendar spread contracts**

The margin requirement for a spread contract does not depend on the price of the spread contract. Instead, margin is computed using the notional size of the contract. For example, the notional size of our BTC futures spread contract is 0.1BTC. Therefore, the margin required for opening a position in the contract a fraction of the USD price of 0.1BTC.

$$Margin = Margin\% * Num\_of\_Contracts * Contract\_Size * Underlying's\_Spot\_Price$$

where, $$Margin\% = 1 / Leverage$$.

That means, higher the leverage, lower Margin% and hence, lower the margin requirement.

A position can be opened at a Margin% that is higher than the Initial Margin% specified the contract's specifications. Also, Just like in futures, margin requirement scales up with position size. Details of Margin Scaling are available [here]({{site.baseurl}}/docs/trading-guide/margin-explainer). 
 

### **Mark Price**

Open positions in calendar spread contracts are marked using Fair Price Marking. The fair price of a calendar spread contract is computed as the difference of the mark prices of the constituent futures contracts. 

### **Profit/ Loss Equation**

The profit/ loss (PNL) equation for a position in a spread contract is essentially the same as for vanilla futures.

For long positions:

$$PNL = Num\_of\_Contracts * Contract\_Size * (Current\_Contract\_Price - Entry\_Contract\_Price)$$

For short positions:

$$PNL = - Num\_of\_Contracts * Contract\_Size * (Current\_Contract\_Price - Entry\_Contract\_Price)$$


Please note that the profit/ loss of our BTC futures spread contract is denominated in USDT.


### **Liquidations**

A position in a calendar spread contract goes into liquidation, when Position Margin after factoring in unrealised losses is less than Maintenance Margin, i.e. 

$$ Position\ Margin + Unrealised PNL = Maintenance\ Margin$$

where, (a) Position Margin is greater than or equal to Initial Margin, and (b) Unrealised PNL is computed at the prevailing Mark Rate.

The liquidation mechanism is exactly the same as for futures contracts. Any given position is liquidated in a step-wise manner to reduce the market impact of liquidations. Details of the liquidation process are available [here]({{site.baseurl}}/docs/trading-guide/Liquidation/#liquidation).

Traders also have the option of enabling [Auto Margin Top-up]({{site.baseurl}}/docs/trading-guide/automargin) to prevent their positions from getting liquidated.

## Trading Fees
For the BTC futures calendar spread contract, both maker and taker fees are set at 0.05%. Trading fees are charged on the notional value of the . 

The trading fees schedule for all the contracts listed on Delta Exchange is a available [here](https://www.delta.exchange/fees).

## Expired Calendar Spread Contracts
The Settlement Prices of expired spread contracts as well as other contracts (MOVE and futures) are available on this [page](https://www.delta.exchange/app/expired_futures).