---
layout: default
title: Interest Rate Swaps Guide
has_children: true
nav_order: 6
---

# Interest Rate Swaps Guide

1. Table of Contents
{:toc}

## Interest Rate Swaps: Motivation & Use Cases
Interest Rate Swaps (IRS) are a class of derivative contracts in which two parties agree to exchange one stream of interest rate payments for another, over a set period of time. Typically, one of the interest rate streams varies with time and is known as as the 'floating rate'. And, the other interest rate stream stays constant and is referred to as the 'fixed rate'. Thus, a typical IRS entails swap of floating interest rate payments with fixed interest payments. 

![image]({{site.baseurl}}/assets/images/IRS.jpg "An Interest Rate Swap")


**Floating rate:**  is typically an easily observable/ benchmark rate that changes periodically and is of interest to a lot of people. 

From the traditional finance world, a good example of such a rate is LIBOR. LIBOR is short for London InterBank Offer Rate. This is the rate at which banks are willing to offer uncollateralised loans to each other. LIBOR is published everyday and is used as the floating leg on a multitude of interest rate swaps and other interest rates based derivatives. In the domain of cryptocurrencies, widely accepted benchmark interest rates like LIBOR are yet to emerge. However, interest rates on de-fi platforms (that typically change with every block on the ETH blockchain) and funding rates on perpetual contracts (that typically change every 8 hours). 


**Fixed rate:**  or the swap rate, is the rate which a party demands in exchange of assuming the uncertainty of paying floating rate over the duration of the IRS. It is logical to deduce that the fixed rate in an IRS will reflect the expected values of the floating rate in the future. 

In traditional financial markets, the market-implied future values of benchmark rates like LIBOR can be imputed from a variety of interest rate derivatives. These 'forward rates' help traders in pricing LIBOR-based interest rate swaps. Unfortunately, market-implied forward rates are not available in crypto yet. Thus, traders will need to come up with their own estimates of future values of the floating rate of an IRS.


**Notional Amount:** can be thought of as the 'Principal Amount' for which fixed or floating interest payments are being exchanged in an IRS. Notional Amount is plugged into the simple interest formula to compute interest rate payments that two parties in an IRS have to make to each other, i.e. 

 $$Interest\ Rate\ Payment = Notional\ Amount * Rate * Time$$ 

**Using IRS for hedging**

You can get into an IRS trade to convert floating-rate exposure/ liability into a fixed rate one. By buying floating-for-fixed, you can eliminate the uncertainty of the floating-rate liability. 

![image]({{site.baseurl}}/assets/images/irs_hedging.jpg "Hedging a floating rate liability with IRS")

**Using IRS to speculate**

You can use IRS to speculate on the future values of the floating rate. 

- **Buy floating-for-fixed: Pay fixed rate, Receive floating rate**

If you believe that the floating rate is likely to go up, you can choose to receive the floating rate and pay the fixed rate. This trade will be profitable if the realised value of the floating rate over the is higher than the expected value baked into the fixed rate.

![image]({{site.baseurl}}/assets/images/irs_trade2.jpg "IRS Trade when you expect floating rate to go up")

- **Sell floating-for-fixed: Pay floating rate, Receive fixed rate**

If you believe that floating rate is likely to do gown, you can choose to pay the floating rate and receive the fixed rate. Since you have locked in a higher fixed rate, your trade will be profitable if your expectation about floating rate going lower comes true. 

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

To be able to understand the profit/ loss calculations for an IRS, it is important for you to appreciate the fact that the profit/ loss from an IRS trade is path dependent. What this means is that it is not just your entry and exit levels but also the path the floating rate takes during the trade that determines your profit/ loss. To put this in context, Profit/ loss for futures is path independent as is evident from the futures PNL equation:

$$PNL = P_{exit} - P_{entry}$$

The path dependency of profit/ loss is due to the fixed-floating payment exchanges that happen while you are in an IRS trade. In fact, thinking in terms of cashflows is a good starting point for computing profit/ loss from an IRS trade. Cashflows can occur: (a) at the trade inception, (b) while the trade is open and fixed/ floating payments are being exchanged periodically and (c) at the time of exit from the trade. You make profit in a trade when the cumulative net cashflow (i.e. total incoming cashflow - total outgoing cashflow) is positive. 


Recall that an IRS is a derivative contract in which two parties agree to exchange fixed/ floating payments for a pre-determined duration of time. When a party is looking to exit from an IRS position before the contract's tenure is complete, it does so by taking opposite position in the same IRS for the remainder of tenure of the IRS. So, if you are receiving fixed/ paying floating, you do a trade in which you pay fixed/ receive floating to close your position. 


As the above diagrams illustrate, on closing an IRS position, you end up paying or receiving the difference of the fixed rates in the two opposing swaps for the remaining duration of the IRS contract.


Therefore, the profit/ loss from an IRS contract trade can be written as:
$$PNL = Cashflow\ at\ incepetion + Net\ fixed/ floating\ payments + Cashflow\ at\ Exit$$ 

## Interest Rate Swaps Listed on Delta Exchange
- **BitMex Funding Rate Swap**
This contracts enables you to swap the funding rate of BitMex's XBTUSD perpetual contract that changes every 8 hours with a rate that stays fixed through the duration of the swap. You can use the BitMex funding rate swap either to hedge the risk of variability of funding you are paying on an open position in BitMex's XBTUSD or to speculate on the movement in funding rate. Complete details of this contract are available [here]({{site.baseurl}}/docs/tutorials/bitmex-funding-swap).

- **Dai Savings Rate Swap**
This contract enables you to swap variable Dai Savings Rate with a rate that stays fixed through the duration of the swap. You can use the Dai Savings Rate swap to both hedge the risk of change in Dai Savings Rate as well as to speculate on Dai and the broader DeFi ecosystem. Complete details of this contract are available [here]({{site.baseurl}}/docs/tutorials/dai-savings-rate-swap).


