---
layout: default
title: Order Types
has_children: false
parent: Margin Trading Guide
nav_order: 3
---

# Order Types

1. TOC
{:toc}

We currently support the following types of orders

## Limit Orders
A limit ordder is an order to buy or sell a specified number of futures contracts at a specified price. A limit order will only ever fill at the specified price or better price. Limit orders trade-off guaranteed execution for lower trading costs. 

```
User inputs: Quantity & Limit Price
Limit order example
Quantity = 50 contracts
Limit price = 4000
Direction = Buy
 ```
 _A buy order for 50 contracts with a limit price of 40000 will be sent to the order book_

## Market Orders
A market is an order to buy or sell a specified number of futures contracts at the best available price available in the order book. There is no guarantee that a market order will fill at any particular price. A market order may fill at a number of different prices, based on the quantity of the market order and the quantities of the existing orders on the order book at the time. Market orders are used when immediate execution is more important than trading costs. 

```
User inputs: Quantity
Market order example
Quantity = 50 contracts
Direction = Buy
```
 _A buy order for 50 contracts will be sent to the order book_

## Stop Orders

Stop orders are conditional orders which become active only after the market reaches a certain Trigger Price (also known as Stop Price). A stop order thus has three states:
- _Untriggered_- Market has not reached the Trigger Price. 
- _Triggered_ - Market has reached the Trigger Price. The stop order has become active and has entered the order book.
- _Filled_ - After getting activated, the stop order has been filled.

It is important to note that currently only **Mark Price** can be used to specify the Trigger Price of stop order.

Delta Exchange currently offers three types of stop orders:

- **Stop market order** - When the stop order is triggered, a market order is sent to the order book.

	```
	User inputs: Stop Price, Quantity
	Stop market order example
	Quantity = 50 contracts
	Stop price = 3800
	Direction = Buy
	```
	_A buy order for 50 contracts will be sent to the order book_
- **Stop limit order** - When the stop order is triggered, a limit order is sent to the order book. 

	```
	User inputs: Stop Price, Limit Price, Quantity
	Stop limit order example
	Quantity = 50 contracts
	Stop price = 3800
	Limit price = 3900
	Direction = Buy
	```

- **Trailing stop order** - In this order type, the stop price follows the market at a fixed distance (known as the Trail Amount) when the market is moving in favour of the trader. Stop price reamins unchanged when the market is moving against the trader. This feature of trailing stop orders enables a trader to specify a limit on the maximum possible loss, without setting a limit on the maximum possible gain. When the market reaches the trigger price, a market order is sent to the other book.

	```
	User inputs: Stop Price, Trail Amount, Quantity
	Trailing stop order example
	Quantity = 50 contracts
	Trail Amount = 40
	Direction = Buy
	```



