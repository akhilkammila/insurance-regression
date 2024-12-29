# Kaggle S4 E12: Insurance Regression

## Journey
1. Basic EDA (eda.ipynb)

Tried finding which variables are correlated with target (not in file anymore)
Found Deotte's post on NA values
    drew pointplots of numeric values VS. average target (since there are few unique vals)
        as well as numeric
    plotted NA on these graphs as well

2. Some models (submissions.py)

Tried to get some basic models running
    Just guessing the mean on LOGGED target (to optimize for RMSLE) yields 1.095 rmsle
    LR model gets similar (1.092)
    XGB (100 estimators) gets 1.057
        only used numeric variables
        NA values not filled (xgb deals with it)
        did not have year columns
    Hist GB (also 100 estimators) gets 1.046
        added year col
        used both numeric and categorical, did not fill NA values (it deals with it)

Made models simpler
    Got similar results (1.05 range) with Hist GB and a single decision tree just using:
        annual income
        health score
        previous claims
        credit score
        customer feedback NA status (1 if NA, 0 otherwise)
    Visualized decision tree, found that Annual Income & Credit Score dominate

3. OOF Strategy (oofnonlog.ipynb and catboost.ipynb) - from backpaker

Strategy:
    first, train a tree model which optimizes for MSE (not RMSE, so not logging target variable)
    then, train a second final model which inputs the first tree's predictions (this time predicting log target)

Offnonlog.ipynb: attepted to do this using HistGBRegressor, failed (same 1.045 results for both)
Catboost.ipynb: replicated for catboost (essentially same exact as backpaker, replicated 1.031 crossval score)
Lgbm.ipynb: tried with light gbm as well


Directions for moving forward:

Feature Engineering
    - add interaction variables (with annual income / credit score)
    - add frequency encoding (tell LGBM how common values are) --> uses some test data as well
        rarer values may be more meaningful for ex.
    - aggregations (ex. for a specific class, what is the average)
        tells LGBM whether value is common or rare for a group

full list:
    - annual income * + - credit score, other relevant vars. (maybe normalize first)
    - look at NA value overlap (potentially add new col which is just isNA)
    - frequency encoding
    - target encoding (aggregations)

Data processing & NaNs
    - for some models like lightGBM, currently leaving NaN values as is
    LGBM treats NaNs specially, leading to overfit
    need to process these better (convert to number) to prevent this

    - check whether categorical or numeric is better (LGBM can handle either)

full list:
    - cv with some different settings:
        is categorical or numeric better
        is NaNs better left as NaNs, or converted to number/category
        removing a lot of vars