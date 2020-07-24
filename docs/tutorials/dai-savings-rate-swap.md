---
layout: default
title: DAI Savings Rate Swap
has_children: false
parent: Interest Rate Swaps Guide
nav_order: 2
---

# Dai Savings Rate Swap

1. Table of Contents
{:toc}


Dai holders can deposit Dai into the Dai Savings Rate contract to earn interest. This is similar to earning interest on your savings account balance. The rate of interest you can earn in Dai savings is known as the Dai Savings Rate (DSR). DSR is a variable rate, and as with other aspects of Maker DAO, the DSR can be changed by Maker's governing body, i.e. MKR token holders. The current value of DSR is 0%.

The DSR also impacts the Stability Fee charged on Dai that is generated from Maker Vaults. Essentially, Maker Protocol envisages Dai as a collateralised loan. Users create Dai by locking up collateral into a Vault. The protocol allows several different asset to be used a collarateral for generating Dai. The interest rate that is charged on borrow of Dai is known as the Stability Fee.

$$Stability Fee = Base Rate + Collateral Rate$$

Stability Fee is a variable rate that is comprised of two parts: 

(a) Base Rate: which can be considered the benchmark rate that applies to all collateral type and, 

(b) Collateral Rate: which captures the risky-ness of a particular asset. More risky (volatile) assets will have a higher Collateral Rate compared to less risky assets. It is worth noting that Collateral Rates are generally quite stable and almost all the variability in the Stability Fee comes from the Base Rate.

Although there is no direct relationship, a good proxy for the Base Rate is the Dai Savings Rate. If DSR goes up/ down by x%, so does the Base. Therefore, even users that generate Dai are exposed to the DSR through Stability Fee.

The Dai Savings Rate swap is a derivative contract that entails swapping the floating (variable) DSR with a fixed rate for the duration of the swap. This contract is relevant for users that earn DSR on their Dai, users that generate Dai from Maker Vaults and for traders interested in speculating on Dai and the broader DeFi ecosystem.


**DAI savings rate swap as a risk management tool**

- **For earners of DSR:** Depositers of the Dai Savings Rate contract assume the risk of DSR going down which will reduce their earnings. They can short the Dai Savings Rate swap, i.e. pay the floating DSR and receive a fixed rate. Thus, entering into the swap enables the depositors to earn a fixed interest rate on their Dai deposits. 

- **For generators of Dai:** Users that create Dai by locking collateral in Maker smart contracts assume the risk of a varaible borrow rate. They can hedge this risk by going long the Dai Savings Rate swap, i.e. pay a fixed rate to receive the floating DSR. Entering in this swap converts the variable rate borrow to an effectively fixed rate borrow.


**DAI savings rate swap as a speculative trading instrument**
 
- **Buy floating-for-fixed:** In this trade, you are paying a fixed rate for a variable stream of DSR. You would do well if saving rate increases after you have locked it in a with a fixed rate. DSR is increased when DAI is trading at a discount. If you believe DAI will start trading at a discount this is the trade for you. 

- **Sell floating-for-fixed:** In this trade, you are getting a fixed rate for paying a variable stream of DSR. You would do well if the DSR is reduced after you have gotten a high fixed rate to swap it. DSR is reduced when there is excessive demand for DAI and it starts trading at a premium to 1 USD. If you believe DAI is likely to go into premium, this is the trade for you. 


## Mechanics of DAI Savings Rate Swap

### **The Swap Currency**

The notional value of the funding rate swap is in Dai. This means that the fixed/ floating payments and margin all are DAI deonminated.

### **Orderbook**

The DAI savings rate swap orderbook has bids and offers in terms of annualised fixed rate. Bid represent the fixed rate a party is willing to offer to buy the stream of DSR over the period of the swap. Similarly, offers represent the fixed rate a party is demanding to sell the stream of DSR over the swap tenure. 

![image]({{site.baseurl}}/assets/images/irs_orderbook.jpg "DAI Savings Rate Swap Orderbook")

### **Fixed/ floating exchange frequency**

Fixed rate payments are made upfront at the trade inception, while the floating (funding) payments happen every 24 hours. 

### **Cost of trading Dai Savings Rate Swap**

Buyers of floating rate are required to pay the complete fixed rate payments upfront to the sellers. We refer to this as Premium. 

$$Premium = Notional\ Value * Fixed\ Rate * (Time\_to\_Maturity/ 31536000)$$

Premium is negative for buyers of floating (i.e. cash outflow) and positive for sellers of floating (i.e. cash inflow).

Additionally, the seller is also required to post margin for the variable funding exchanges that will take place while the parties are holding their positions.

**Margin for sellers of floating rate**

$$Seller's\ margin = Notional\ Value * Margin\% * Rate\ Term$$

where Rate Term is computed as:

$$Rate\ Term = (- Fixed\ Rate - max (Max\_Funding\_Rate, 0) * (Time\_to\_Maturity/ 31536000)$$

Here, Margin% is the required margin ratio. To initiate a position, InitialMargin% is required and to keep it open MaintenanceMargin% is required. 

Just like in futures, margin requirement scales up with position size. Details of Margin Scaling are available [here]({{site.baseurl}}/docs/trading-guide/margin-explainer). 
 

### **Mark Rate**

Like our futures contracts, open positions in DAI savings rate swap contracts are marked using Fair Price Marking. The fair price, rather fair rate, for the DAI savings rate swap is the Impact Mid Rate. In cases, when the percentage spread between Impact Ask Rate and Impact Bid Rate is more the MaintenanceMargin%, we consider the order book to be illiquid. Mark Price is not updated when the order book is illiquid.

### **Profit/ Loss Equation**

There are three types of cashflows that happen when you are trading the funding rate swap:

**Premium:** This is the cashflow that takes places at the inception of the swap. Premium is paid by buyer of floating rate to the seller of floating rate.

**Floating payments:** These occur every 24hours and are made by seller of floating to buyers of floating.

$$Floating\ payment\ for\ buyers\ of\ floating\ rate = Notional\ Value * Funding\_Rate * (1/365)$$

**Pay-off:** The cashflow that occurs when a position in an IRS contract is closed is referred to as Pay-off. As explained earlier in this guide, closing an IRS position is akin to doing a trade in the opposite direction of your existing position. This means, Pay-off can be computed using the same formula as for Premium.

The PNL from a position in Dai Savings Rate swap is the sum of the cashflows, i.e. 


$$ PNL = Premium + Floating\ Payments +  Pay-off$$


### **Liquidations**

Longs (i.e. buyers of floating) pay the fixed payments upfront as Premium. During the course of the swap, longs only receive floating payments from seller and have no liabilities. Due to this reasons, a long position can never get liquidated.

A short position in the DSR swap goes into liquidation, when Position Margin after factoring in unrealised losses is less than Maintenance Margin, i.e. 

$$ Position\ Margin + Pay-off = Maintenance\ Margin$$

where, (a) Position Margin is greater than or equal to Initial Margin, and (b) Pay-off is computed at the prevailing Mark Rate.

The liquidation mechanism is exactly the same as for futures contracts. Any given position is liquidated in a step-wise manner to reduce the market impact of liquidations. Details of the liquidation process are available [here]({{site.baseurl}}/docs/trading-guide/liquidation).

Shorta also have the option of enabling [Auto Margin Top-up]({{site.baseurl}}/docs/trading-guide/automargin) to prevent their positions from getting liquidated.

## Trading Fees
For the DSR swap, maker and taker fees are 0.05% and 0.10% respectively. Trading fees are charged on the notional value of the swap. 

The trading fees schedule for all the contracts listed on Delta Exchange is a available [here](https://wwww.delta.exchange/fees).


## Expired IRS Contracts
The Settlement Prices of expired IRS as well as other contracts (MOVE and futures) are available on this [page](https://www.delta.exchange/app/expired_futures).

