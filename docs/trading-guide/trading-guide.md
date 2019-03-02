---
layout: default
title: Trading Guides
has_childred: true
nav_order: 3
permalink: /docs/trading-guide
---

# Trading Guide

1. TOC
{:toc}

## Trade lifecycle

### Funding exchange wallet

Bitcoin is the only funding currency on Delta Exchange. This means that you can deposit bitcoins to your Delta wallet and withdraw only bitcoins from your Delta wallet. After logging on to Delta Exchange, you need to go to you **Account** page to get your bitcoin deposit address and to learn how to withdrawl bitcoin from Delta Exchange.

### Currency conversion
Even though bitcoin is the only funding currency on Delta Exchange, not all futures contracts are settled in bitcoin. For example, we have stablecoin-settled contracts where the margining and settlement happens in USDC. Therefore, you'd need USDC for trading these futures contracts. Since we currently don't support deposit/ withdrawal of USDC (or for that matter any other crypto apart from bitcoin), we have provided a **currency converter tool** that enables users to change bitcoin to the desired cryptocurrency.

It is worth noting that the exchange rate offered in the currency conversion is in line with the prevailing rates at the top spot exchanges. Further, currency conversion doesn't attract any trading fees.

### Placing an order

Orders can be placed in Place Order Panel in the trading dashboard. We currently support three types of order:

  -   **Limit order:** is an order to buy or sell a specified number of derivative contracts at a specified price. A limit order will only ever fill at the specified price or better price.
    
-   **Market order:** is an order to buy or sell a specified number of derivative contracts at the best available price available in the order book. There is no guarantee that a market order will fill at the price specified. A market order may fill at a number of different prices, based on the quantity of the market order and the quantities of the existing orders on the order book at the time.
- **Stop order:** is an order which is triggered at a stop price provided by the user.
    
### Margin & PnL Calculations

A new order is allowed to be submitted to the exchange only if the trader has sufficient balance available to reserve the order margin. Order margin computation happens in one of the two ways:

-   Trader doesn’t have an existing position in the contract: the system computes the initial margin that would be required if this position is acquired. This is the amount that is blocked as order margin.
    
-   Trader has an existing position in the contract: in this case, the system computes the margin required for the updated overall position after the placed order has been executed. The difference between this computed margin requirement and the current position margin is what is additionally needed for this order. This is the amount that is block as order margin.
    
Details on the various types of margins and their calculations are available [here](#marginexplain).

Existing positions on Delta are marked at [fair Price](#fpm). This means that your unrealised PnL and hence the current value of margin allocated to a particular position are a function of the marked price. PnL calculations are illustrated with example in the [PnL Math](#pnlmath) section.

### Settlement

You can square off a position in a derivatives contract in the exchange. Position that are held till maturity are cash settled at a price that is computed using the settlement method described in the [contract specifications](https://wwww.exchange/contracts).

  

### Maintenance & Market Disruption Events

Trading can be halted for scheduled maintenance and in case of unanticipated events that have the potential to disrupt trading.

 - **Scheduled maintenance:** trading suspension happens in two phases. Phase 1: Order book is put in cancel-only mode and no new orders are accepted. Phase 2: Order book is completely frozen and, new order or cancellations are accepted and no matches occur.
 - **Market disruption events:** These could include unavailability of spot price from one or more exchanges that contribute to the underlying index, uncharacteristically high price volatility among other things. In addition to suspension of trading activity, market disruption events could also impact contract maturities and settlement

 When it is time to restart trading activity, market is put in post-only mode. New maker orders will be accepted, but no matches will occur. Trading will resume once there is sufficient liquidity in the order book 

  

## Fair Price Marking

Leverage is inherent in derivative contracts, combined with high volatility of crypto-currencies can lead to unwarranted liquidations. Lack of liquidity could further exacerbate this situation as price swings in a derivatives contract relative to the underlying index could widen even further.

To ensure a great trader experience, Delta marks all open position at Fair Price instead of Last Traded Price. The Fair Price has lesser volatility and is more resilient against attempts to manipulate market.

It is worth noting that Fair Price marking is relevant only computation of Unrealized PnL and Liquidation price. Realized PnL is computed using actual trading prices and is thus not impacted by Fair Price.

### Calculation of Fair Price of a Futures Contract

The price of a futures contract converges to the underlying index price at the time of contract maturity, i.e.

$$Futures\_Price = Underlying\_Index Price$$

At all other times, the price of a futures contract broadly moves in tandem with the price of the Underlying Index, with the difference between the two referred to as basis, i.e.

$$Basis = Futures\_Price - Underlying\_Index\_Price$$

Since the Underlying Index is the foundation of the futures contract, it is logical to assume that

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

The $$Underlying\_Index\_Price$$ is obviously independent of the trading happening on Delta Exchange and is sourced in real-time from leading spot exchanges.

### Impact Price

To understand the computation of $$Fair\_Basis$$, we first need to introduce the notion of Impact Price. This price tries to estimate the price at which a typical long or short position (called Impact Position) in the futures contract can be entered at any given time.

Impact Position, in terms of number is contracts to be traded, is provided in the specifications for each futures contract. It is easy to see that Impact Price is a function of: (a) Impact Position and (b) current state of the order book.

$$Impact\_Bid\_Price = \text{Average fill price to execute a typical short trade}$$

$$Impact\_Ask\_Price = Average \ Fill \ Price \  to \  execute \ a \ typical \ long \ trade$$

$$Impact\_Mid\_Price = Average (Impact\_Bid\_Price, Impact\_Ask\_Price)$$
 

  
### Fair Basis Calculation

We first compute an annualised fair value basis rate, $$\%Fair\_Basis$$:

$$\%Fair\_Basis = (Impact\_Mid\_Price/ Underlying\_Index\_Price - 1) * (365*86400/ time\_to\_expiry\_in\_sec)$$

$$\%Fair\_Basis$$ is computed only once every minute. Further, in case at any time of update, market is illiquid, i.e. 

$$(Impact\_Ask\_Price - Impact\_Bid\_Price) > Maintenance\_Margin$$, 

$$\%Fair\_Basis$$ is not updated.

$$\text{Now, } Fair\_Basis = Underlying\_Index\_Price * \%Fair\_Basis * (time\_to\_expiry\_in\_sec/ (365* 86400))$$

Now, that we have the $$Fair\_Basis$$, the fair price of the futures (i.e. the mark price) can be easily computed:

$$Futures\_Fair\_Price = Underlying\_Index\_Price + Fair\_Basis$$

It is worth noting that only live positions are marked using the Fair Price. Thus, unrealised PnL may swing with the Mark Price, realised PnL is determined using actual traded price and is unimpacted by Mark Price.

## <a id="marginexplain"></a>Margining Explainer

Margin is the collateral that you need to post when entering into a leveraged derivatives contract. The amount required to enter into a new position is referred to as Initial Margin, which is dependent on the leverage offered in the derivatives contract.

If the trade against you, the unrealised loss in your position is adjusted against the initially posted margin. When the remaining margin (Initial Margin - Unrealised Loss) goes below Liquidation Margin, [liquidation process](#liquidation) is triggered.

 Delta Exchange follows a **Segregated Margin approach**, in which margin is explicitly assigned to each position and is not shared across positions. A position is specific to a particular contract. Thus, for each contract, two margin sub-accounts are maintained:

- **Position Margin:** Margin allocated to all existing (may include multiple long/ short positions) in given derivatives contract

- **Order Margin:** Margin allocated to all open orders in a given derivatives contract

It is worth noting that Unrealized PnL is not factored into Position Margin or Order Margin.

Position Margins and Order Margins are allocated from your Wallet Balance. Thus, at any time, the amount that remains unallocated is what is available for placing a new order. This is referred to as Available Balance.

$$Available\  Balance = Wallet\  Balance - (Position\  Margin + Order\  Margin)$$

Everytime a new order is placed, the system does three things:

 1. computes the Reservation margin for this new order,
 2. checks whether $$Reservation \ Margin <= Available \ Balance$$, and
 3. if $$2$$ holds, then transfer Reservation Margin amount from Available Balance to the Order Margin$ of the contract.

  

### Reservation Margin Computation

Initial Margin (IM) requirements for a standalone order are as follows:
-   Buy limit order

$$IM = (Initial\ Margin\% * \#Contracts * Multiplier * Limit\_Bid\_Price)$$
    
-   Buy market order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * MarkPrice)$$
    
-   Sell limit order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * Max (Limit\ Offer\ Price, BestBid)$$
    
-   Sell market order

$$IM = (InitialMargin\% * \#Contracts * Multiplier * Max (MarkPrice, BestBid)$$
    

Now, if there are existing positions/ open orders in the contract, then the Initial Margin requirement for the new combination of position and open orderss is recomputed. For this computation, positions or orders on opposite side are netted in such a manner that for two offsetting orders, margin is reserved only once.

Reservation  Margin is then the difference of the Initial Margin requirement for the combined position (existing + new order) and the Position Margin and Order Margin currently allocated to the derivative contract.

### Impact on Margins on order cancellations

In a given contract, Order Margin account contains margin blocked for all the current open orders. If one or more of these open orders is cancelled, the Initial Margin requirement for the remaining open orders and existing orders is recomputed. The new Initial Margin requirement will either be same as earlier or lower. If case of latter, excess margin is released.

## <a id="pnlmath"></a>PnL Math

All open positions on Delta are marked at the [Fair Price](#fpm) of the Futures contract. Thus, Unrealized PnL and Liquidiation Prices are computed using Fair Prices, while Realized PnL is based on actual entry and exit prices.

PnL for a long/ short position in a Vanilla Futures

$$PnL = ± n*m*(Fut\_CurrentPrice - Fut\_EntryPrice)$$

PnL for a long/ short position in an Inverse Futures

$$PnL = ± n*m*(1/ Fut\_EntryPrice - 1/ Fut\_CurrentPrice)$$

 where $$m$$ is the multiplier and $$n$$ is the position size (i.e. number of contracts).

If a position is acquired at multiple entry prices, an average entry price is computed and used for PnL computation.

## <a id="liquidation"></a>Liquidation
Each position on Delta has an associated prices:

-   Liquidation Price: At Liquidation Price, the difference of Position Margin minus Unrealized PnL of the position is equal to the Liquidation Margin.
    
-   Bankruptcy Price: At Bankruptcy Price, the Unrealized Loss of a position equal to the Position Margin.
    
When Liquidation Price of a position is hit, liquidation of this position is initiated. It is worth noting that since Delta Exchange uses Segregated Margins, liquidation of position in a particular contract has no bearing on other existing positions and open orders on other derivative contracts.

Liquidation mechanism on Delta Exchange is comprised of the following steps:

-   All open orders on the derivative contract are cancelled. This may or may not free up some margin blocked for these order.
    
-   An Immediate-or-Cancel order is submitted to the market to close the positions in the contract. The limit price of this order is set to the Bankruptcy Price. This ensures that the Realised Loss will always be lower than the Position Margin.
    
-   If the liquidation results in being filled at price better than the Bankruptcy Price, then the user keeps whatever remaining Position Margin there is.
    
-   In case the position is not fully liquidated (for example due to lack of liquidity), the remainder of the position is terminated at Bankruptcy Price and ADL is triggered.
    

**Liquidation Examples**
You have an open long position of $$1000$$ contracts in Bitcoin-Dollar Futures with an Average Entry Price of $$10000$$. The Liquidation Price for this position is $$9500$$ and the Bankruptcy Price is $$9200$$.

As soon as the Mark prices reaches $$9500$$ or below, your position will enter Liquidation. An immediate-or-cancel sell order is submitted on your behalf at $$9200$$. The limit price of this order assures that if the order is matched, the portfolio value of the margin account will not be less than zero.

**Case 1:** Liquidation order gets filled at $$9400$$. At this price since your margin is not fully eroded, the remaining margin is released.

**Case 2:** The Liquidation order at gets only partially filled and say 300 contracts are left over. This will trigger [ADL](#autodel). The system will find counterparties on the short side and a trade between you and these counterparties for $$300$$ contracts at $$9200$$  (i.e. the Bankruptcy Price) is made to happen.

  

## <a id="autodel"></a>Auto deleveraging

Auto deleveraging (ADL) is initiated if liquidation of a position is doesn’t get completed before the Mark  Price reaches the Bankruptcy Price of the position. This means that the Liquidation order (which is an immediate-or-cancel order at Bankruptcy Price) is either completely or partially unfilled. In this situation, the left over contracts from the position under liquidation are forcefully matched (at the Bankruptcy Price) with counterparties on the opposite side.

 It is worth noting that all the open orders of the deleveraged counterparties in the same contract are also cancelled. Those deleveraged are notified via email and are given an opportunity to re-enter their positions.

  **Selection of ADL counterparties**

ADL counterparties are selected on the basis of profits (in $$\%$$ terms) of their positions. All open positions are ranked according to their $$Profit\%$$, with the position with the highest $$Profit\%$$ on top. Deleveraging starts from the top position and continues to deleverage the subsequent positions until all the leftover contracts from the Liquidation order which led to ADL are matched.

 You can gauge the likelihood of your position being selected as an ADL counterparty using the ADL Indicator. This indicator informs in which quintile your position falls when all open positions in your side (long or short) are sorted by $$Profit\%$$. Obviously, positions in the top (bottom) quintile are most (least) likely to get deleveraged.

 **ADL Example**

In Dollar-Bitcoin Futures contract, there are 7 longs, and their positions sizes and $$Profit\%$$ are as follows:

| Account 	| #Contracts 	| Unrealised PnL 	| Profit% Rank 	| Quintile 	|
|:---------:| :------------:| :---------------:|:--------------:|:---------:|
|    1    	|     100    	|      -10%      	|       6      	|     1    	|
|    2    	|     20     	|       20%      	|       1      	|     5    	|
|    3    	|     50     	|       5%       	|       3      	|     4    	|
|    4    	|     80     	|      0.2%      	|       4      	|     3    	|
|    5    	|      5     	|       15%      	|       2      	|     5    	|
|    6    	|     30     	|      -20%      	|       7      	|     1    	|
|    7    	|     70     	|       -7%      	|       5      	|     2    	|

Let’s assume a short position is to be closed via ADL. We will start with the Account which has the highest $$Profit\% \ Rank$$ and continue to move to following Accounts till we have the requisite number of long contracts to match the short position being liquidated.

 **Case 1:** The short position has $$15$$ contracts. In this case, Account 2 will have $$15$$ of its $$20$$ contracts matched against this short position at the Bankruptcy Price of the short position. No other accounts will be impacted.

**Case 2:** The short position has $$40$$ contracts. In this case, $$20$$ contracts of Account 1, $$5$$ contracts of Account 5 and $$15$$ contracts of Account 3 will be deleveraged.
