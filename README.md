# Project 2 #

## Project Proposal ##

![Screenshot](Wally's_2.png)

### Group 3 - Investment Wallys ###


Wallys SAAS Widgets is a made up company that gets its revenue from subscription sales.
These subscriptions are paid for in either USDC, ETH or WBTC crypto currencies. 

Our objective is to build a solution to manage their treasury. For short term payments, salaries and other outgoings, Wally's needs to keep a low risk, liquid threshold of 40% USDC in its treasury with the remaining 60% made up of crypto coins.  Our trading bot manages the buy and sell of crypto currencies to keep the funds of USDC at optimum levels for operating expenses.  

The Wally's accounts team have created their own version of an automated market maker. In our model, our trading bot will iterate over the last twelve months of data using our proprietry trading algorythms. The daily OHLCV data sets are sourced from the Coinbase Exchange platform via our Alpaca trading account. Focused only on the daily data, we first analyse the most profitable strategy for the last twelve months. 

Using indicators including Moving Average Convergence/Divergence (MACD), MYC Trading Indicator, Relative Strength Index (RSI), Bollinger Bands and Moving Averages (MA), we will determine based on our historical data,the best moments to “buy” and “sell” (trading signals) of our chosen trading strategy. 
We then test to find the best predictive model from one of  predictive models: Desion tree, Prope
t, AdaBoost, SVC and the Linear Regression models. 


After training different classification models using the training data, we compare accuracy, precision and recall of different models and determine the model with the optimum performance for the next month. The results of the trading algorithm with best outcome we will later separate on features X (information about the price fluctuation and indicators) and predictions y (trading signals). Further the data will be split  on the training and testing sets. We will scale data. 

After refining our strategy, we use the model to forecast the investment position as at the end of the month for our finance team. We also use the model to refined strategy. Similar to an exponential model, we have observed that the best results are produced from a data set consisting of a twelve month period.


![Screenshot2](Screenshot2.png)

At the end of the month, the balance of our successful trading period is accounted for in our Tresury account. Subject to the volitatly in the selected crypto price, our month-end  balance should sit at 40% USD with a reliably predicted increase in our holdings of crypto currencies.  During the month the bot will continue to trade using the optimal combination of indicators for our trading strategy. We also use the prediction model to provide a mid month forcast for our financial reporting. This strategy of check, scale, test and predict should yeild a consistant and reliable result month on month with minimal risk acceptable to our ever trusting partners. 

## Brief of Classiers used.


In Vicki's case, we have used the MACD and signal-line as the 'Predictors' and the signal as the 'Target' set using the folowing Classifiers.

## SVM - Support Vector Machine 
We used the SVM classifiers which is a regression techinique that maximises the predictive accurancy without overfitting the training data. 

![Screenshot](Screenshot_SVM1.png)
![Screenshot](Screenshot_SVM1.png)
![Screenshot](Screenshot_SVM1.png)

## Logistic Regression
THis is usuafull where there are a volume of features of little value and can be ignored. It is a supervised learning technnique that is good at predicitng the probability of events based on dependant variables. Not all data fits a model cleanly which is why regression is good to analyzis the relationship between the variables.

![Screenshot](Screenshot_Logistic1.png)
![Screenshot](Screenshot_Logistic1.png)

## AdaBoost
"Ada" or Adaptive Boosting was used to assess the accuracy and output data also. This works by adjusting or reassigning the weights to instances which reduces the bias in data sets. Accuracy after weight-reassigning and re-ittereating increases.

![Screenshot](Screenshot_ADABoost1.png) 
![Screenshot](Screenshot_AdaBoost2.png)

## Decision Tree
The Decision Tree clssifier identifies the best predictor (Root Node) after breaking the data down to develop smaller and smaller subsets. Entropy and Information Gain are used to construct the decision tree. Entropy is used to calulate the homogeneity of the different attributes. Information Gain identifies the most homogeneous branches which returns the highest information gain. 

![Screenshot](Screenshot_Decision_Tree1.png)










## Credits

## Git Manager# 
Robert

## Data pre-processing, Alpaca Data - 3 years #
Karin
Vicky

## Trading bot code and Alpaca #
Leigh
Robert
Ilia

## Prophet Model #
Ilia
Vicky

## Linear Regression Model #
Karin
Vicky

## Visual Representations - new Library Candlestick chart #
Karin
Vicky


## Presentation preparation #
Karin
Vicky
