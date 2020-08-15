# Bitcoin Volatility Arbitrage Trading 
A tool used to analyze volatility arbitrage opportunities in cryptocurrency derivative markets. 

#### Disclaimer: This tool is only for demonstration purposes and is not financial advice. This script has not undergone extensive testing and is prone to errors and bugs. Use this tool at your own risk. 

## Overview
Option contracts are powerful financial instruments to execute a variety of different trading strategies. Notably, the popular long straddle trade is a combination of buying a call and put with the same strike and maturity. A trader would purchase a straddle if they expect future volatility to be higher based on their forecast. In this case, the trader doesn’t care whether the price of an asset goes up or down. Rather the trader cares about ***how*** fast the price moves as they wish to profit from an increase in volatility (either direction). 

Deribit Exchange offers the greatest option market liquidity relative to the other players in this space. Using the calls and puts trading on Deribit, we can create our own straddle by going long the respective calls and puts. 

FTX is another prominent exchange which offers a product called MOVE contracts. These MOVE contracts are quite similar to straddles in the sense their value is derived from the “[absolute value of the amount a product moves in a period of time](https://help.ftx.com/hc/en-us/articles/360033136331-MOVE-contracts#:~:text=What%20are%20MOVE%20contracts%3F,BTC%20went%20up%20or%20down).” 

In theory an FTX MOVE contract with a similar maturity should be priced closely in line to the respective Deribit at-the-money straddle. After some observation it became quite surprising that the price differences between these two venues were too large to overlook.

## Key Parameters
- Discuss thresholds and how to use the script

## Example
In the example below, the script is setup to show how to analyze arbitrage opportunites for the chosen FTX MOVE contract.
``` python
>>> import crypto_vol as cv

# List out all MOVE contracts trading on FTX
>>> move_contracts = cv.FTX().get_move_contracts()
>>> move_contracts
['BTC-MOVE-0815',
 'BTC-MOVE-0816',
 'BTC-MOVE-WK-0821',
 'BTC-MOVE-WK-0828',
 'BTC-MOVE-WK-0904',
 'BTC-MOVE-WK-0911',
 'BTC-MOVE-2020Q3',
 'BTC-MOVE-2020Q4',
 'BTC-MOVE-2021Q1']

# Choose whichever FTX MOVE contract you would like to analyze
# Here we will take a look at 'BTC-MOVE-WK-0828'
# You will need to adjust the `strike_threshold` and `days_threshold` accordingly
>>> vol = cv.VolArb('BTC-MOVE-WK-0828', strike_threshold=200, days_threshold=2)

# Retrieve all comparable Deribit Options to form an equivalent straddle
>>> data = vol.get_comparable_deribit()
>>> data
# Returns a dataframe with all the respective options
"""
option_type	strike	  instrument_name	     ftx_expiry_days  deribit_expiry_days option_price
put	       12000.0	  BTC-28AUG20-12000-P	 13.132293	       12.46561	         549.799060
call	     12000.0	  BTC-28AUG20-12000-C	 13.132293	       12.46561          482.544639
"""

# Compare the FTX MOVE and Deribit Straddle 
>>> vol.compare(data)
# Returns a dataframe with printout of the price difference between the two contracts
# Please see attached notebook for a more comprehensive example with a full dataframe output

"""-33.695 % price differential between FTX MOVE and similar Deribit straddle"""
```
Below we can manually see how to assess trading opportunity. 





## Risks and Assumptions Involved
- Not a truly risk-free trade
- Counterparty Risk
- Assumption that trades placed at mid-price
- Margin Called on Trades
- Liquidity Risk 

## Installation of dependencies
Run the following command in terminal to install all of the required packages.

```
pip install -r requirements.txt
```
