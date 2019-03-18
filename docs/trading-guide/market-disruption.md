---
layout: default
title: Market Disruption
has_children: false
parent: Margin Trading Guide
nav_order: 7
---

# Market Disruption

Trading in one or more futures contracts can be halted for scheduled and unscheduled maintenance and in case of unanticipated events that have the potential to disrupt trading.

 - **Scheduled and unscheduled maintenance:** We will announce any scheduled maintenance at least 24 hours in advance. Trading suspension prior to maintenance periods happens in two phases:
 1. Phase 1: Order book is put in cancel-only mode and no new orders are accepted. Thus, traders have the option to cancel any existing orders.
 2. Phase 2: Order book is completely frozen and, new order or cancellations are accepted and no matches occur.
 - **Market disruption events:** Unavailability of spot price from one or more exchanges that contribute to the underlying index may impair our ability to mark open positions correctly, forcing us to suspend trading. Additionally, exceptional situations like uncharacteristically high price volatility may also warrant suspension of trading. It is important to note that market disruption events could also impact contract maturities and settlement.

## Why Unavailability of Spot is a Market Disruption Event

At Delta Exchange we use [fair price marking](#fair-price-marking). This means that all open positions on Delta Exchange are marked at Mark Price, which in turn depends on the spot price. Thus, unavailability of spot price results in inability to mark open positions correctly.
  
## Trading Resumption Process
Post a trading suspension event, We use a single price auction to resume normal trading. This happens in two steps:
- **Step 1:** Market is put into post-only mode. Traders are able to post and/ or cancel limit orders. However, at time time, orders are not matched.
- **Step 2:** After sufficient liquidity has built up in the order book, the state of the order book is frozen momentarily. During this time, news orders are not accepted and existing orders cannot be cancelled. A single price at which maximum number of buy/ sell orders can be matched is computed. This is known as the open price. After open price has been calculated and where possible, orders have been matched at the open price, normal trading resumes.