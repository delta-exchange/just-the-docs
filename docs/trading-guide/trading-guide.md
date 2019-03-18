---
layout: default
title: Margin Trading Guide
has_children: true
nav_order: 3
permalink: /docs/trading-guide
---

# Trade Life Cycle

1. TOC
{:toc}

## Funding exchange wallet

Bitcoin is the only funding currency on Delta Exchange. This means that you can deposit bitcoins to your Delta wallet and withdraw only bitcoins from your Delta wallet. After logging on to Delta Exchange, you need to go to your [Account Page](https://www.delta.exchange/app/account) to get your bitcoin deposit address and to learn how to withdrawl bitcoin from Delta Exchange.

## Currency conversion
Even though bitcoin is the only funding currency on Delta Exchange, not all futures contracts are settled in bitcoin. For example, we have stablecoin-settled contracts where the margining and settlement happens in USDC. Therefore, you'd need USDC for trading these futures contracts. Since we currently don't support deposit/ withdrawal of USDC (or for that matter any other crypto apart from bitcoin), we have provided a **currency converter tool** that enables users to change bitcoin to the desired cryptocurrency.

It is worth noting that the exchange rate offered in the currency conversion is in line with the prevailing rates at the top spot exchanges. Further, currency conversion doesn't attract any trading fees.

## Placing an order

Orders can be placed in Place Order Panel in the trading dashboard. We currently support three types of order:

  -   **Limit order:** is an order to buy or sell a specified number of derivative contracts at a specified price. A limit order will only ever fill at the specified price or better price.
    
-   **Market order:** is an order to buy or sell a specified number of derivative contracts at the best available price available in the order book. There is no guarantee that a market order will fill at the price specified. A market order may fill at a number of different prices, based on the quantity of the market order and the quantities of the existing orders on the order book at the time.
- **Stop order:** is an order which is triggered at a stop price provided by the user.
    
## Margin & PnL Calculations

A new order is allowed to be submitted to the exchange only if the trader has sufficient balance available to reserve the order margin. Order margin computation happens in one of the two ways:

-   Trader doesn’t have an existing position in the contract: the system computes the initial margin that would be required if this position is acquired. This is the amount that is blocked as order margin.
    
-   Trader has an existing position in the contract: in this case, the system computes the margin required for the updated overall position after the placed order has been executed. The difference between this computed margin requirement and the current position margin is what is additionally needed for this order. This is the amount that is block as order margin.
    
Details on the various types of margins and their calculations are available [here](#margining-explainer).

Existing positions on Delta are marked at [fair Price](#fair-price-marking). This means that your unrealised PnL and hence the current value of margin allocated to a particular position are a function of the marked price. PnL calculations are illustrated with example in the [PnL Math](#pnl-math) section.

## Settlement

You can square off a position in a derivatives contract in the exchange. Position that are held till maturity are cash settled at a price that is computed using the settlement method described in the [contract specifications](https://wwww.exchange/contracts).