# Project 2 #

## Project Proposal ##

![Screenshot](Wally's_2.png)

### Group 3 - Investment Wallys ###


Wallys SAAS Widgets is a made up company that gets its revenue from subscription sales.
These subscriptions are paid for in either USDC, ETH or WBTC crypto currencies. 

Our objective is to build a solution to manage their treasury. For short term payments, salaries and other outgoings, Wally's needs to keep a low risk, liquid threshold of 40% USDC in its treasury with the remaining 60% made up of crypto coins.  Our trading bot manages the buy and sell of crypto currencies to keep the funds of USDC at optimum levels for operating expenses.  

The Wally's accounts team have created their own version of an automated market maker. In our model, our trading bot will iterate over the last twelve months of data using our proprietry trading algorythms. The daily OHLCV data sets are sourced from the Coinbase Exchange platform via our Alpaca trading account. Focused only on the daily data, we first analyse the most profitable strategy for the last twelve months. We then test to find the best predictive model from one of four predictive models: Desion tree, AdaBoost, SVC and the Linear Regression models. 

We then use the best predictor based on accracracy to predict the signal in our data sets.After refining our strategy, we use the model to forecast the investment position as at the end of the month for our finance team. We also use the model to refined strategy. Similar to an exponential model, we have observed that the best results are produced from a data set consisting of a twelve month period.

At the end of the month, the balance of our successful trading period is accounted for in our Tresury account. Subject to the volitatly in the selected crypto price

![Screenshot2](Screenshot2.png)

By the end of the month the balance should sit at 40% USD and 60% crypto currencies.  During the month the bot will trade the currencies.  The operations the bot performs should be as successful as possible.
 
We will collect data for 3 years to model and predict and analyse the success of our bot.  

We will use Prophet and Linear Regression for predictions. 


Credits

# Git Manager# 
Robert

# Data pre-processing, Alpaca Data - 3 years #
Karin
Vicky

# Trading bot code and Alpaca #
Leigh
Robert
Ilia

# Prophet Model #
Ilia
Vicky

# Linear Regression Model #
Karin
Vicky

# Visual Representations - new Library Candlestick chart #
Karin
Vicky


# Presentation preparation #
Karin
Vicky

# Amazon Lex #
Ilia
Robert
