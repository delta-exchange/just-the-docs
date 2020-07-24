---
layout: default
title: BitMex Funding Rate Swap
has_children: false
parent: Interest Rate Swaps Guide
nav_order: 1
---

# BitMex Funding Rate Swap

1. Table of Contents
{:toc}

The BitMex Funding swap is defined over the BitMex Bitcoin Perpetual Swap (XBTUSD) funding rate. For a perpetual contract, funding is the mechanism which tethers the price of the contract to the spot price. Broadly speaking, when the perpetual contract is trading at a premium to spot, i.e. more buying pressure, longs are required to pay funding to shorts. Conversely, when the perpetual contract is trading at a discount to shorts, shorts are required to pay funding to longs.

On BitMex, funding rate is comprised of two parts: premium of orderbook derived price over spot price and (b) differential of USD and BTC lending rate. The lending rates have been set to fixed numbers. Thus, funding rate largely depends on premium. Over an 8-hour period premium is measured and averaged to get the funding rate, which is then exchanged at the end of the next 8-hour period. Everyday funding is exchanged at 4am UTC, 12pm UTC and 8pm UTC, between traders who have a position in the contract at that time.

![image]({{site.baseurl}}/assets/images/xbt_funding_infographic.jpg "BitMex XBTUSD Funding Rate Characteristics")


The XBTUSD funding rate can thought of as an interest rate that can vary from -493% to +493% annualised and changes every 8 hours. In this context, we have created an interest rate swap contract with the BitMex XBTUSD funding as the floating rate. We refer to this contract as the BitMex funding rate swap. It can be used by traders for both risk management and speculation.

**BitMex funding rate swap as a risk management tool**

XBTUSD funding rate is generally positive, i.e. more often than not, longs pay shorts, and stays in the 15-20% annualised band. However, it is also prone to violent moves, especially around big run-ups and sharp sell-offs. If you intend to hold a position in XBTUSDT for an extended period of time, funding can potentially eat significant part of your profits from price move. In such situations, you can get into a BitMex funding swap trade to convert the floating funding exposure to a fixed one.

If you are long XBTUSD perpetual, then you can buy floating-for-fixed in the BitMex Funding Swap for the same notional size as your XBTUSD position. By doing this, you'd pay a fixed rate to get the funding paid by longs in XBTUSD perpetual. The net result of these trades is that you have swapped the uncertainty of variable funding into a fixed rate.

Conversely, if you are short XBTUSD perpetual, then you a sell floatin-for-fixed in the BitMex Funding Swap for the same notional size as your XBTUSD position. By doing this, you'd get a you'd get a fixed rate and will have to pay the funding received by shorts in XBTUSD perpetual. Here too, these trade change the exposure to variable funding rate to a a fixed rate.

Note that above trades have been explained with an implicit assumption that funding rate is positive. When funding is negative, the direction of floating payments will reverse, but the hedge will stay intact. Let's consider a long XBTUSD position hedged with BitMex funding rate swap (i.e. you have paid a fixed rate to get funding paid by longs). If funding rate goes negative, you'd receive funding on your XBTUSD position. And, you'd need to pay the same funding rate to your counterparty in the funding rate swap. So change in funding rate doesn't impact your PNL.

**BitMex funding rate swap as a speculative trading instrument**

You can trade the BitMex funding rate swap without having a position in XBTUSD. The magnitude and sign of XBTUSD is often a barometer for market sentiment. Funding is high and positive, when market is bullish. Conversely, in bearish times, funding is usually in negative territory. Depending up on your view/ situation, you can get into the following trades to speculate on XBTUSD funding:

- **Buy floating-for-fixed:** In this trade, you are paying a fixed rate for a variable stream of funding rates. You would do well if funding rate increases after you have locked it in a with a fixed rate. This trade would make sense when sentiment is likely to turn from bearish to bullish or from bullish to more bullish.

- **Sell floating-for-fixed:** In this trade, you are getting a fixed rate for paying a variable stream of funding rates. You would do well if funding rate decreases after you have gotten a high fixed rate to swap it. This trade would make sense when sentiment is likely to turn from bullish to bearish or from bearish to more bearish.


## Mechanics of BitMex Funding Rate Swap

### **The Swap Currency**

BitMex's XBTUSD perpetual is an inverse contract. The contract's underlying is BTC; it is quoted in USD and is margined and settled in BTC. To ensure that the funding rate swap can be used to hedge funding incurred on XBTUSD positions, we have followed the same convention.

The notional value of the funding rate swap is in USD. This means that the fixed/ floating payments too should be denominated in USD. However, margining and all cashflows happen in BTC. This is achieved by converting USD to BTC using the prevailing spot rate at the time a cashflow takes place.

### **Orderbook**

The BitMex funding rate swap orderbook has bids and offers in terms of annualised fixed rate. Bid represent the fixed rate a party is willing to offer to buy the stream of funding rates over the period of the swap contract. Similarly, offers represent the fixed rate a party is demand to sell the stream of funding rates over the swap tenure. 

![image]({{site.baseurl}}/assets/images/irs_orderbook.jpg "BitMex Funding Rate Swap Orderbook")

Since, funding is quoted as an 8hour rate on BitMex, we give an option to see the orderbook in terms of 8hour rates. However, all trading happens in annualised rates.

### **Fixed/ floating exchange frequency**

Fixed rate payments are made upfront at the trade inception, while the floating (funding) payments happen every 8 hours (4am UTC, 12pm UCT and 8pm UTC), in sync with funding exchanges on BitMex. 

The next funding payment in known 8 hours in advance. You would pay/ receive funding only if you hold a position at the funding payment times.

### **Cost of trading BitMex Funding Rate Swap**

Buyers of floating rate are required to pay the complete fixed rate payments upfront to the sellers. We refer to this as Premium. 

$$Premium = (Notional\ Value/ BTCUSD) * Fixed\ Rate * (Time\_to\_Maturity\ 31536000)$$

Premium is negative for buyers of floating (i.e. cash outflow) and positive for sellers of floating (i.e. cash inflow).

Additionally, both buyer and seller are also required to post margin for the variable funding exchanges that will take place while the parties are holding their positions.

**Margin for buyers of floating rate**

$$Buyer's\ margin = (Notional\ Value/ BTCUSD) * Margin\% * Rate\ Term$$

where Rate Term is computed as:

$$Rate\ Term = (Fixed\ Rate - min (Min\_Funding\_Rate, 0))* (Time\_to\_Maturity/31536000)$$

**Margin for sellers of floating rate**

$$Seller's\ margin = (Notional\ Value/ BTCUSD) * Margin\% * Rate\ Term$$

where Rate Term is computed as:

$$Rate\ Term = (- Fixed\ Rate - max (Max\_Funding\_Rate, 0) * (Time\_to\_Maturity/ 31536000)$$

Here, Margin% is the required margin ratio. To initiate a position, InitialMargin% is required and to keep it open MaintenanceMargin% is required. 

Just like in futures, margin requirement scales up with position size. Details of Margin Scaling are available [here]({{site.baseurl}}/docs/trading-guide/margin-explainer). 
 

### **Mark Rate**

Like our futures contracts, open positions in BitMex funding rate swap contracts are marked using Fair Price Marking. The fair price, rather fair rate, for the funding rate swap is derived by leveraging the fact that cashflows of a position in funding rate swap can be approximated by taking positions of the same size but opposite directions in the XBTUSD perpetual and a BTC futures expiring at the same time as the funding rate swap. This means the annualised rate imputed from the basis of the BTC futures can be considered as the market's expectation of the funding rates from now till the time of futures/ swap expiration. 

We therefore use the annualised implied rate from BTC futures basis as fair rate. 

### **Profit/ Loss Equation**

There are three types of cashflows that happen when you are trading the funding rate swap:

**Premium:** This is the cashflow that takes places at the inception of the swap. Premium is paid by buyer of floating rate to the seller of floating rate.

**Funding payments:** These occur every 8hours, in sync with funding exchanges between XBTUSD position holders on BitMex. Whether you pay or receive funding payment, depends both on your position and sign of funding rate. 

$$Funding\ for\ buyers\ of\ floating\ rate = (Notional\ Value/ BTCUSD) * Funding\_Rate * (1/1095)$$

$$Funding\ for\ sellers\ of\ floating\ rate = - (Notional\ Value/ BTCUSD) * Funding\_Rate * (1/1095)$$

**Pay-off:** The cashflow that occurs when a position in an IRS contract is closed is referred to as Pay-off. As explained earlier in this guide, closing an IRS position is akin to doing a trade in the opposite direction of your existing position. This means, Pay-off can be computed using the same formula as for Premium.

The PNL from a position in BitMex funding rate swap is the sum of the cashflows, i.e. 


$$ PNL = Premium + Funding\ Payments +  Pay-off$$


### **Liquidations**

A position in the BitMex funding rate swap goes into liquidation, when Position Margin after factoring in unrealised losses is less than Maintenance Margin, i.e. 

$$ Position\ Margin + Pay-off = Maintenance\ Margin$$

where, (a) Position Margin is greater than or equal to Initial Margin, and (b) Pay-off is computed at the prevailing Mark Rate.

The liquidation mechanism is exactly the same as for futures contracts. Any given position is liquidated in a step-wise manner to reduce the market impact of liquidations. Details of the liquidation process are available here.

Traders also have the option of enabling [Auto Margin Top-up]({{site.baseurl}}/docs/trading-guide/automargin) to prevent their positions from getting liquidated.

## Trading Fees
For the BitMex funding rate swap, maker and taker fees are 0.05% and 0.10% respectively. Trading fees are charged on the notional value of the swap. 

The trading fees schedule for all the contracts listed on Delta Exchange is a available [here](https://wwww.delta.exchange/fees).


## Expired IRS Contracts
The Settlement Prices of expired IRS as well as other contracts (MOVE and futures) are available on this [page](https://www.delta.exchange/app/expired_futures).

