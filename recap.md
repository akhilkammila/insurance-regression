# Competition Recap and Reflection

## Information and Background
- Competition: https://www.kaggle.com/competitions/playground-series-s4e12
- First kaggle comp, spent ~1 week
    - looked at discussions after a couple days

- Type of data
    - predicting insurance premium based on several features, none of them seem to stand out
    - NA values are slightly different, some categorical variable splits are different (but not by that much)
    - RMSLE scores are over 1 (this means that predictions are on average more than 2.7x off)

## Competition Recap

1. Own Findings
- we tried to find correlations, but did not see much

2. NA values --> Modelling
- after looking at Deotte's post about NA values, we plotted pointplots and explored splitting by categorical variables vs. NA
(https://www.kaggle.com/competitions/playground-series-s4e12/discussion/552165)

- based on backpaker (https://www.kaggle.com/competitions/playground-series-s4e12/discussion/550350), we tried
    - FIRST fit a model to predict actual insurance price based on RMSE (not log)
    - then use the predictions of the first model, to predict RMSLE (now logged)

3. What did we learn?
- we did very little feature engineering
- we mainly used modelling techniques (catboost on H100bs to fit)

- final 1st place solution (https://www.kaggle.com/competitions/playground-series-s4e12/discussion/554328)
seems to EXHAUSTIVELY SEARCH many INTERACTION TERMS in a robust manner

- uses Target Encoding, Count Encoding, and checks many combinations of features