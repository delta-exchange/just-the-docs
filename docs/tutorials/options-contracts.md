---
layout: default
title: Options Guide
has_children: true
nav_order: 5
---

# Options Guide
Options are a class of derivtive contracts that give a buyer a right to buy or sell an underlying asset at a specified price prior to or on a specified date. The seller of the options contract has the corresponding obligation to fulfill the transaction (i.e. to sell or buy) if the buyer "exercises" the option.

At Delta Exchange, we offer the following three categories of options contracts:

- **Vanilla Options**

 In this category we offer call and put options on BTC, ETH, BNB and LINK. All these are European options (i.e. they can only be exercised on expiry) and have 1 day and 7 day expiries.

- **MOVE Options**

MOVE options are a direct way to speculate on the volatility of the underlying assets. A MOVE contract is essentially a straddle, i.e. an at-the-money call & put option pair. Because of this, the price of a MOVE contract is proportional to the price swings in the underlying, instead of the direction of underlying's price movement. More details on MOVE options are available [here]({{site.baseurl}}/docs/tutorials/move-contracts).

- **Turbo Options**

Turbo options are exotic options in which a knockout barrier is attached to vanilla call/ put options. Just like a call option, a Turbo call option increases in value when price of the underlying goes up. And, like put options, a Turbo put option increase in value when price of the underlying goes down. It is the knockout barrier which is differentiates Turbo options from vanilla options. Vanilla options have a fixed expiry date, whereas a Turbo option may expire (or get knocked out) before its expiry date if the underlying's price touches the knockout barrier.More details on Turbo options are available [here]{{site.baseurl}}/docs/tutorials/turbo-options).