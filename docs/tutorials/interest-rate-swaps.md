---
layout: default
title: Interest Rate Swaps Guide
nav_order: 5
---

# Interest Rate Swaps Guide

1. Table of Contents
{:toc}

## Interest Rate Swaps: Motivation & Use Cases
Interest Rate Swaps (IRS) are a class of derivative contracts in which two parties agree to exchange one stream of interest rate payments for another, over a set period of time. Typically, one of the interest rate streams varies with time and is known as as the 'floating rate'. And, the other interest rate stream stays constant and is referred to as the 'fixed rate'. Thus, a typical IRS entails swap of floating interest rate payments with fixed interest payments. 

![image]({{site.baseurl}}/assets/images/swap.jpg "An Interest Rate Swap")


**Floating rate:**  is typically an easily observable/ benchmark rate that changes periodically and is of interest to a lot of people. 

From the traditional finance world, a good example of such a rate is LIBOR. LIBOR is short for London InterBank Offer Rate. This is the rate at which banks are willing to offer uncollateralised loans to each other. LIBOR is published everyday and is used as the floating leg on a multitude of interest rate swaps and other interest rates based derivatives. In the domain of cryptocurrencies, widely accepted benchamrk interest rates like LIBOR are yet to emerge. However, interest rates on de-fi platforms (that typically change with every block on the ETH blockchain) and funding rates on perpetual contracts (that typically change every 8 hours). 


**Fixed rate:**,also known as the swap rate, is the rate which a party demands in exchange of assuming the uncertainty of paying floating rate over the duration of the IRS. It is logical to deduce that the fixed rate in an IRS will reflect the expected values of the floating rate in the future. 

In traditional financial markets, the market-implied future values of benchmark rates like LIBOR can be imputed from a variety of interest rate derivatives. These 'forward rates' help traders in pricing LIBOR-based interest rate swaps. Unfortunately, market-implied forward rates are not available in crypto yet. Thus, traders will need to come up with their own estimates of future values of the floating rate of an IRS.


**Notional Amount:** can be thought of as the 'Principal Amount' for which fixed or floating interest payments are being exchanged in an IRS. Notional Amount is plugged into the simple interest formula to compute interest rate payments that two parties in an IRS have to make to each other, i.e. 

 $$Interest\ Rate\ Payment = Notional\ Amount * Rate * Time$$ 

**Using IRS for hedging**
You can get into an IRS trade to convert floating-rate exposure/ liability into a fixed rate one. By buying floating-for-fixed, you can eliminate the uncertainty of the floating-rate liability. 

![image]({{site.baseurl}}/assets/images/IRS_hedge.jpg "Hedging a floating rate liability with IRS")

**Using IRS to speculate**
You can use IRS to speculate on the future values of the floating rate. 

**Buy floating-for-fixed: Pay fixed rate, Receive floating rate**

If you believe that the floating rate is likely to go up, you can choose to receive the floating rate and pay the fixed rate. This trade will be profitable if the realised value of the floating rate over the is higher than the expected value baked into the fixed rate.

![image]({{site.baseurl}}/assets/images/irs_trade2.jpg "IRS Trade when you expect floating rate to go up")

**Sell floating-for-fixed: Pay floating rate, Receive fixed rate**

If you beleive that floating rate is likely to do gown, you can choose to pay the floating rate and receive the fixed rate. Since you have locked in a higher fixed rate, your trade will be profitable if your expectation about floating rate going lower comes true. 

![image]({{site.baseurl}}/assets/images/irs_trade1.jpg "IRS Trade when you expect floating rate to go down")


## Understanding the Math Behind IRS

In an IRS,

$$Value\ of\ Floating\ Payments\ = Notional\ Amount* \sum(floating\ rate_i * T_i)$$

where, $$floating\ rate_i$$ is the value of the floating rate in the i-th time interval. Obviously, these refer to future values of the floating rate and thus need to be estimated by the traders.

$$Value\ of\ Fixed\ Payments = Notional\ Amount* fixed\ rate * \sum T_i$$

For a fairly priced IRS, we have must have:

$$Value\ of\ Fixed\ Payments\ = Value\ of\ Floating\ Payments$$

Using the above equation, we can find the fair value of the fixed rate as:

$$fixed\ rate = \sum (floating\ rate_i * T_i) / \sum T_i$$

It is worth noting the following:

1. In the above equations, we have ignored time value of money, i.e. the present value of futures payments is not getting impacted by their timing. This approximation helps keep the calculations simple and does not have material impact on IRS of short tenures.
2. We have assumed that fixed and floating payment exchanges are synchronized. That need not be the case. In fact, in the IRS contracts listed on Delta Exchange, fixed payment is done in a single shot at the inception of the swap.

**Profit/ loss Calculations**

To be able to understand the profit/ loss calcuations for an IRS, it is important for you to appreciate the fact that the profit/ loss from an IRS trade is path dependent. What this means is that it is not just your entry and exit levels but also the path the floating rate takes during the trade that determines your profit/ loss. To put this in context, Profit/ loss for futures is path independent as is evident from the futures PNL equation:

$$PNL = P_{exit} - P_{entry}$$. 

The path dependency of profit/ loss is due to the fixed-floating payment exchanges that happen while you are in an IRS trade. In fact, thinking in terms of cashflows is a good starting point for computing profit/ loss from an IRS trade. Cashflows can occur: (a) at the trade inception, (b) while the trade is open and fixed/ floating payments are being exchanged periodically and (c) at the time of exit from the trade. You make profit in a trade when the cumulative net cashflow (i.e. total incoming cashflow - total outgoing cashflow) is positive. 


Recall that an IRS is a derivative contract in which two parties agree to exchange fixed/ floating payments for a pre-determined duration of time. When a party is looking to exit from an IRS position before the contract's tenure is complete, it does so by taking opposite position in the same IRS for the remainder of tenure of the IRS. So, if you are receiving fixed/ paying floating, you do a trade in which you pay fixed/ receive floating to close your position. 


As the above diagrams illustrate, on closing an IRS position, you end up paying or receiving the difference of the fixed rates in the two opposing swaps for the remaining duration of the IRS contract.


Therefore, the profit/ loss from an IRS contract trade can be written as:
$$PNL = Cashflow\ at\ incepetion + Net fixed/ floating\ payments + Cashflow\ at\ Exit$$ 

## BitMex Funding Rate Swap

The BitMex Funding swap is defined over the BitMex Bitcoin Perpetual Swap (XBTUSD) funding rate. For a perpetual contract, funding is the mechansim which tethers the price of the contract to the spot price. Broadly speaking, when the perpetual contract is trading at a premium to spot, i.e. more buying pressure, longs are required to pay funding to shorts. Conversely, when the perpetual contract is trading at a discount to shorts, shorts are required to pay funding to longs.

On BitMex, funding rate is comprised of two parts: premium of orderbook derived price over spot price and (b) differential of USD and BTC lending rate. The lending rates have been set to fixed numbers. Thus, funding rate largely depends on premium. Over an 8-hour period premium is measured and averaged to get the funding rate, which is then exchanged at the end of the next 8-hour period. Everyday funding is exchanged at 4am UTC, 12pm UTC and 8pm UTC, between traders who have a position in the contract at that time.


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

The notional value of the funding rate swap is in USD. This means that the fixed/ floating payments too should be denominated in USD. However, margining and all cashflows happen in BTC. This is achieved by converting USD to BTC using the prevailaing spot rate at the time a cashflow takes place.

###**Orderbook**

The BitMex funding rate swap orderbook has bids and offers in terms of annualised fixed rate. Bid represent the fixed rate a party is willing to offer to buy the stream of funding rates over the period of the swap contract. Similarly, offers represent the fixed rate a party is demand to sell the stream of funding rates over the swap tenure. 

Since, funding is quoted as an 8hour rate on BitMex, we give an option to see the orderbook in terms of 8hour rates. However, all trading happens in annualised rates.

### Fixed/ floating exchange frequency

Fixed rate payments are made upfront at the trade inception, while the floating (funding) payments happen every 8 hours (4am UTC, 12pm UCT and 8pm UTC), in sync with funding exchanges on BitMex. 

The next funding payment in known 8 hours in advance. You would pay/ receive funding only if you hold a position at the funding payment times.

### Cost of trading BitMex Funding Rate Swap

Buyers of floating rate are required to pay the complete fixed rate payments upfront to the sellers. We refer to this as Premium. 

$$Premium = (Notional\ Value/ BTC\ spot\ price) * Fixed\ Rate * (Time\_to\_Maturity\ 31536000)$$

Premium is negative for buyers of floating (i.e. cash outflow) and positive for sellers of floating (i.e. cash inflow).

Additionally, both buey and seller are also required to post margin for the variable funding exchanges that will take place while the parties are holding their positions.

**Margin for buyers of floating rate**

$$Buyer margin = (Notional\ Value/ BTC\ spot\ price) * Margin\% * (Fixed\ Rate - min (Min\_Funding\_Rate, 0))* (Time\_to\_Maturity/31536000)$$

**Margin for sellers of floating rate**

$$Seller margin = (Notional\ Value/ BTC\ spot\ price) * Margin\% (- Fixed\ Rate - max (Max\_Funding\_Rate, 0) * (Time\_to\_Maturity/ 31536000)$$

Here, Margin% is the required margin ratio. To initiate a position, InitialMargin% is required and to keep it open MaintenanceMargin% is required. 

Just like in futures, margin requirement scales up with position size. Details of Margin Scaling are available [here]({{site.baseurl}}/docs/trading-guide/margin-explainer). 
 

### Mark Rate

Like our futures contracts, open positions in BitMex funding rate swap contracts are marked using Fair Price Marking. The fair price, rather fair rate, for the funding rate swap is derived by leveraging the fact that cashflows of a position in funding rate swap can be approximated by taking positions of the same size but opposite directions in the XBTUSD perpetual and a BTC futures expriring at the same time as the funding rate swap. This means the annualised rate imputed from the basis of the BTC futures can be considered as the market's expectation of the funding rates from now till the time of futures/ swap expiration. 

We therefore use the annualised implied rate from BTC futures basis as fair rate. 

### Profit/ Loss Equation

There are three types of cashflows that happen when you are trading the funding rate swap:

**Premium:** This is the cashflow that takes places at the inception of the swap. Premium is paid by buyer of floating rate to the seller of floating rate.

**Funding payments:** These occur every 8hours, in sync with funding exchanges between XBTUSD position holders on BitMex. Whether you pay or receive funding payment, depends both on your position and sign of funding rate. 

$$Funding for buyers of floating = (Notional\ Value/ BTC\ spot\ price) * Funding\_Rate * (1/1095)

$$Funding for sellers of floating = - (Notional\ Value/ BTC\ spot\ price) * Funding\_Rate * (1/1095)

**Pay-off:** The cashflow that occurs when a position in an IRS contract is closed is referred to as Pay-off. As explained earlier in this guide, closing an IRS position is akin to doing a trade in the opposite direction of your existing position. This menas, Pay-off can be computed using the same formuala as for Premium.

The PNL from a position in BitMex funding rate swap is the sum of the cashflows, i.e. 


$$ PNL = Premium + Funding\ Payments +  Pay-off$$


### Liquidations

A position in the BitMex funding rate swap goes into liquidation, when Position Margin after factoring in unrealised losses is less than Maintenance Margin, i.e. 

$$ Position\ Margin + Pay-off = Maintenance\ Margin$$

where, (a) Position Margin is greater than or equal to Initial Margin, and (b) Pay-off is computed at the prevailing Mark Rate.

The liquidation mechanism is exactly the same as for futures contracts. Any given position is liquidated in a step-wise manner to reduce the market impact of liquidations. Details of the liquidation process are available here.

Traders also have the option of enabling [Auto Margin Top-up]({{site.baseurl}}/docs/trading-guide/automargin) to prevent their positions from getting liquidated.

## Trading Fees
For the BitMex funding rate swap, both maker and taker fees are 0.15%. Trading fees are charged on the notional value of the swap. 

The trading fees schedule for all the contracts listed on Delta Exchange is a available [here](https://wwww.delta.exchange/fees).


## Expired IRS Contracts
The Settlement Prices of expired IRS as well as other contracts (MOVE and futures) are available on this [page](https://www.delta.exchange/app/expired_futures).

