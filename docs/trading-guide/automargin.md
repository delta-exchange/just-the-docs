---
layout: default
title: Auto Margin Top-up
has_children: false
parent: Margin Trading Guide
nav_order: 5
---

# Auto Margin Top-up

1. TOC
{:toc}

[Segregated Margin]({{site.baseurl}}/docs/trading-guide/margin-explainer/#margining-explainer) is the default margining mode on Delta Exchange. This means that an open position has access to only the margin assigned to that position. Consequently, an open position may go into liquidation if the position margin erodes, even if there is sufficient balance available in a user's account. This behaviour can be modified through the use of **Auto Margin Top-up**. 

Auto Margin top-up is a feature that enables traders to automatically add margin to open positions in order to avoid liquidation. If Auto Margin Top-up is enabled for a position, whenever the position is about to go into liquidation Position Margin is replenished. 

Recall that a position goes into [liquidation]({{site.baseurl}}/docs/trading-guide/Liquidation/#liquidation) when the Remaining Position Margin (i.e. Initial Margin - Unrealised Losses) is equal to or lower than the Maintenance Margin. The quantum by which margin for a position is topped up each time is computed as follows:
 - **For contracts where maximum leverage is 100x:** margin added to the position is equal to (minimum Initial Margin - Maintenance Margin), where minimum Initial Margin is the margin required to open the position at the highest allowed leverage.
 - **For contracts where maximum leverage is <100x:** margin added to the position is equal to 0.5 times (minimum Initial Margin - Maintenance Margin), where minimum Initial Margin is the margin required to open the position at the highest allowed leverage.

 If the Available Balance is less than the amount required for the top-up, then all of the Available Balance will be used to replenish the margin of the position. It is also important to note that Auto Margin Top-up cannot convert currency on its own and can draw margin only it is available in the settlement currency. For example, if the position that is about to go into liquidation required USDC margin and you only have BTC in your available balance, margin replenishment will fail. 

 
## How to activate/ deactivate Auto Margin Top-up 

Auto Margin Top-up can be enabled/ disabled by toggling a switch which is available with the entry of which open position. You also have the option to have Auto Margin Top-up activated by default for all positions you acquire. This can be enabled by checking Auto-margin Top-up option in the [preferences section](https://www.delta.exchange/app/account/preferences).

![image]({{site.baseurl}}/assets/images/automarginswitch.jpg "Auto Margin Top-up On/Off Switch")

## How is Auto Margin Top-up different from Cross Margin

In Cross Margin approach, margin is shared between open positions. At any time, the margin assigned to a position is equal to the minimum maintenance margin (i.e. position leverage is always the highest allowed). Whenever needed, a position draws more margin from the Available Balance to avoid liquidation. 

Segregated Margin plus Auto Margin Top-up has similar properties as well but with two key differences:
- Unlike Cross Margin, you have the flexibility to open and maintain your position at a leverage fo your choice
- In Cross Margin, margin drawn by a position to avoid liquidation will be released automatically if the position swings into profit. The same however will not happen automatically in the case of Segregated Margin plus Auto Margin Top-up. Once margin is added to a position, it can only be withdrawn manually.




