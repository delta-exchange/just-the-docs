---
layout: default
title: DeFi Index
has_children: false
nav_order: 7
---

# DeFi Index

1. Table of Contents
{:toc}

## Introduction
DEFI Index is composed of the top ten Defi tokens listed on Delta Exchange. Defi Index tracks market performance of a basket of Defi tokens. Index price is calculated using weighted average real time prices of component tokens on Delta Exchange. It is quoted in USDT.

## Components

Chainlink (LINK) , Aave (AAVE), Yearn.finance protocol (YFI) , Uniswap protocol token (UNI) , Ren (REN), Kyber network token (KNC), Balancer token (BAL), Band protocol (BAND) , Synthetix network token (SNX ), Compound (COMP)

## Index Specification


| Term | Value	| 
|:---------:| :------------:| 
|    Index Symbol |     DEFIUSDT  |     
|    Index Name   	|   DeFi Index   |       
|    Quote Currency   	|     USDT          |  
|   Start Date   	|        | 
|   Base Value   	|     100         | 
| Universe | DeFi Coins listed on Delta Exchange |
| Rebalacing Frequency | Monthly |


## Construction Methodology

Defi Index is made of 10 components. These components are rebalanced at the end of every month. Weights of components depend on the market cap and 30 day volume of the respective components.

Weight of each token in the index is the average of capitalization and liquidity weight. Market cap weight is calculated as percentage share of the market cap of a taken compared to the market cap of index. Similarly, volume weight is also the percentage share of the volume of a taken compared to the total volume of all tokens in the index.

$$Weight = ( Capitalization Weight + Liquidity Weight)/2$$

$$Capitalization Weight = (Market Cap of the token)/ (sum of the market cap of all the tokens)$$

$$Liquidity Weight = (30 day volume of the token)/ (sum of the 30 day volume of all the tokens)$$

If the capitalization or liquidity weight is greater than 30%, the weight is limited to 30%. Therefore, none of the component tokens can get weight greater than 30%.

## Rebalancing

Index is rebalanced at the end of every month. During rebalancing, new components can be added or existing components can be excluded according to the following criteria:

1. If a new defi token listed on Delta exchange gets higher ranking based on the capitalization and liquidity, it will be added to the index. Top 10 tokens in the rankings will be part of the index.
2. In special situation events like soft/hard fork, where token gets split, token may be excluded based on the capitalization and liquidity ranking.
3. In case any component token ceases to trade on Delta Exchange, it will be excluded from the DEFI Index.

## Current Weights


| Coin | Mkt Cap (M USD) | Volume (M 30D) | Cap Wt | Liquidity Wt | Adj Cap Wt | Adj Liquidity Wt | Index Wt |
|:----:| :--------------:| :-------------:| :----: | :----------: | :--------: | :--------------: | :------: | 
| LINK | 5135 | 46383 | 59% | 50% | 30% | 30% | 30.00% |     
| AAVE | 717 | 14184 | 8% | 15% | 14% | 18% | 16.01%   |
| UNI  | 677 | 13616 | 8% | 15% | 13% | 17% | 15.26%   |
| YFI  | 558 | 14401 | 6% | 16% | 11% | 18% | 14.59%   |
| COMP | 442 | 3547  | 5% | 4%  | 9%  | 5%	| 6.58%    |
| SNX  | 486 | 2551  | 6% | 3%  | 10% | 3%  | 6.37%    |
| REN  | 301 | 1483	 | 3% | 2%  | 6%  | 2%  | 3.89%    |
| BAND | 126 | 2852  | 1% | 3%  | 2%  | 4%  | 3.04%    |
| KNC  | 185 | 1226  | 2% | 1%  | 4%  | 2%  | 2.59%    |
| BAL  | 85  | 1314  | 1% | 1%  | 2%  | 2%  | 1.67%    |

