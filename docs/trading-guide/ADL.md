---
layout: default
title: Auto-Deleveraging
has_children: false
parent: Margin Trading Guide
nav_order: 8
---

# Auto-Deleveraging

In derivatives trading, you are trading against counter-parties who are also trading on leverage. All outstanding longs offset against shorts and vice versa such that net open interest remains zero at all times. This is the principle that all derivatives exchanges follow globally to ensure fair and balanced trading between counterparties. 

Whenever a position in liquidation is taken over by the Liquidation Enginer, it tries to close this position at the bankruptcy price or better price. Though rarely, but it is possible that the liquidation engine is not able to close the position, which is in liquidation, in the open order-book. This can happen when the market has moved too fast and beyond the bankruptcy price of the position in liquidation. In such cases counterparties on the opposite side can get auto-delevered (ADL-ed) i.e. their position will also be closed in order to keep the net OI balanced. Parties who get ADL are selected based on the criteria mentioned below.

 It is worth noting that all the open orders of the deleveraged counterparties in the same contract are also cancelled. Those deleveraged are notified via email and can choose to re-enter their positions.

  **Selection of ADL counterparties**

ADL counterparties are selected on the basis of profit and leverage of their positions. ADL rank is computed using the following equations:

$$ADL Rank = PnL% * Position Leverage; if PnL% >0
              = PnL% / Position Leverage; if PnL% <0
where,
PnL% = (Current_Position_Value - Entry_Position_Value)/abs(Entry_Position_Value)
Position Leverage = abs (Mark Price)/ (Mark Price - Bankruptcy Pirce)
$$

All open positions are ranked according to their ADL Ranking, with the position with the highest ADL Rank on top. Deleveraging starts from the top position and continues to deleverage the subsequent positions until all the leftover contracts from the Liquidation order which led to ADL are matched.

 You can gauge the likelihood of your position being selected as an ADL counterparty using the ADL Indicator. This indicator informs in which quintile your position falls when all open positions in your side (long or short) are sorted by $$Profit\%$$. Obviously, positions in the top (bottom) quintile are most (least) likely to get deleveraged.

 **ADL Example**

In Dollar-Bitcoin Futures contract, there are 7 longs, and their positions sizes, $$PnL%$$ and Position Leverage are as follows:

| Account 	| #Contracts 	| PnL% 	| Position Leverage 	| ADL Rank 	|
|:---------:| :------------:| :---------------:|:--------------:|:---------:|
|    1    	|     100    	|      -10%      	|       2      	|     3    	|
|    2    	|     10     	|       20%      	|       1.5     |     6    	|
|    3    	|     50     	|       5%       	|       3      	|     5    	|
|    4    	|     80     	|      0.2%      	|       1.6    	|     4    	|
|    5    	|     20     	|       15%      	|       2.2    	|     7    	|
|    6    	|     30     	|      -20%      	|       4      	|     2    	|
|    7    	|     70     	|       -7%      	|       1.8     |     1    	|

Let’s assume a short position is to be closed via ADL. We will start with the Account which has the largest ADL Rank and continue to move to following Accounts till we have the requisite number of long contracts to match the short position being liquidated.

 **Case 1:** The short position has $$15$$ contracts. In this case, Account 5 will have $$15$$ of its $$20$$ contracts matched against this short position at the Bankruptcy Price of the short position. No other accounts will be impacted.

**Case 2:** The short position has $$40$$ contracts. In this case, $$20$$ contracts of Account 5, $$10$$ contracts of Account 2 and $$10$$ contracts of Account 3 will be deleveraged.