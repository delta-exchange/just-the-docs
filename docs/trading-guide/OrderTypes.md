---
layout: default
title: Order Types
has_children: false
parent: Margin Trading Guide
nav_order: 3
---

1. TOC
{:toc}

# Order Types
We currently support the following types of orders

## Limit Orders
A limit order is an order to buy or sell a specified number of futures contracts at a specified price. A limit order will only ever fill at the specified price or better price. Limit orders trade-off guaranteed execution for lower trading costs. 

```
User inputs: Quantity, Limit Price
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

Stop orders are conditional orders which become active only after the market reaches a certain Trigger Price (also known as Stop Price). Stop orders serve as key risk management tools for traders, enabling them to limit losses on open trades. In addition to that, stop orders can also provide conditional entry into new positions. 

To enable users to place multiple stop orders with ease, margin for a stop order is blocked only when it is triggered. This means that if there isn't sufficient margin available when a stop order is triggered, it will be cancelled.  

It is important to note that currently only **Mark Price** can be used to specify the Trigger Price of a stop order. In the case of a Buy stop order, the stop price must be below the current Mark Price. Correspondingly, for a Sell stop order, the stop price must be above the current Mark Price.

A stop order thus has three states:
- _Untriggered_- Market has not reached the Trigger Price. 
- _Triggered_ - Market has reached the Trigger Price. The stop order has become active and has entered the order book.
- _Filled_ - After getting activated, the stop order has been filled.

Delta Exchange currently offers three types of stop orders:

- **Stop market order** - When the stop order is triggered, a market order is sent to the order book.

	```
	User inputs: Stop Price, Quantity
	Stop market order example
	Quantity = 50 contracts
	Stop price = 3800
	Direction = Buy
	```
	_A buy market order for 50 contracts will be sent to the order book when Mark Price goes above 3800_
- **Stop limit order** - When the stop order is triggered, a limit order is sent to the order book. 

	```
	User inputs: Stop Price, Limit Price, Quantity
	Stop limit order example
	Quantity = 50 contracts
	Stop price = 3800
	Limit price = 3750
	Direction = Buy
	```
	_A buy limit order for 50 contracts with a limit price of 3750 will be sent to the order book when Mark Price goes above 3800_

- **Trailing stop order** - In this order type, the stop price follows the market at a fixed distance (known as the Trail Amount) when the market is moving in favour of the trader. However, stop price remains unchanged when the market is moving against the trader. This feature of trailing stop orders enables a trader to specify a limit on the maximum possible loss, without setting a limit on the maximum possible gain. When the market reaches the trigger price, a market order is sent to the other book.

	```
	User inputs: Stop Price, Trail Amount, Quantity
	Trailing stop order example
	Quantity = 50 contracts
	Trail Amount = 40 (Trail Amount would be negative for Sell orders)
	Direction = Buy
	```
	_For this order, the Trigger Price will follow the Mark Price at a distance of 40 when the Mark Price is going down. And, a Buy market order of 50 contracts will be sent to the order book when Mark Price rises by 40._

## Bracket Orders (deprecated)

** Please use reduce-only orders to create the functionality offered by bracket orders.**

A bracket order lets a trader to 'bracket' any given order with two opposite-side orders, i.e. take-profit and stoploss order. The take-profit order is a market order which is triggered at a pre-defined level and aims to lock-in profits. And, the stoploss order is a stop market order to limit losses. When one of these two orders (take-profit or stoploss) gets executed, the other order will automatically get cancelled.

A bracket order can be placed along with a buy or sell order. In this case, the take-profit and stoploss orders are placed in the order book as soon as the main order is executed. Alternatively, a bracket order can be placed for a position that is already open. In either case, bracket orders are inextricably linked to an open position. As the position sizes changes, the quantity specified in the take-profit and stoploos orders changes in tandem. 

```
User inputs: Quantity, Limit Price, Take-Profit Level, Stoploss Level
Bracket order example
Quantity = 50 contracts
Limit price = 4000
Take-profit Level = 4500
Stoploss level = 3800
Direction = Buy
 ```
_First a limit order to buy 50 contracts at a limit price of 4000 would be sent to the order book. Once this order is executed, two new orders would be created: (a) a take-profit market order to sell 50 contracts with a trigger price of 4500 and (b) a stop market order with a Trigger Price of 3800._

# Advanced Attributes for Orders

## Order Validity Attributes

- **Immediate or cancel (IOC)**
	
	An order which is marked as IOC is executed immediately and any unfilled portion of the order is cancelled. All market orders are by default marked as IOC. Limit orders can be marked as IOC to guarantee immediate execution at the specified price or better. However, if the order cannot be filled completely, the unfilled portion will be cancelled. 

- **Fill or Kill (FOK)**
	
	An order which is marked as FOK is executed immediately and completely or not all at. This implies that the order must be filled in its entirety or cancelled. 

- **Good till cancelled (GTC)**
	
	An order which is marked as GTC remains in effect until it is executed or cancelled by the trader. All limit orders are by default marked as GTC.

## Order Execution Attributes

- **Post only**
	
	A limit order that is marked as post only is accepted into the order book only it would not be immediately match with an existing order in the order book. Marking a limit order post-only ensures that: (a) the order adds to the liquidity rather than takes liquidity from the order book, and (b) the traders will earn a maker rebate when the order is executed.  


- **Reduce only**
	
	An order that is marked as reduce only can only reduce a position and would be cancelled if it would result in increasing the position. While the reduce only flag can be used with any order type, it is most useful when combined with stop orders. A reduce only stop order will only close an existing position and thus can serve as stop loss order for a position.



