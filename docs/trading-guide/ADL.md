---
layout: default
title: Auto-Deleveraging
has_children: false
parent: Margin Trading Guide
nav_order: 5
---

# Auto-Deleveraging

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