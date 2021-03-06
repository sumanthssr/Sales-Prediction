Sales-Prediction
---------------------
#salesprediction #timeseriesanlaysis #traditionalforecasttechnique #featureengg #regression #stackingensemblemodels 


Description
------------------
Predict next 21 days sales of food/non-food items using 3 years historical data.

Algorithm
------------------
There are two stages involved in this case study namely prerun and masterun.

Prerun
----------------
cleaning historical data(master data)
1) Load master data and remove if there are any duplicate records for all items.

Feature engineering 
1) Add the forecasting variables to master data like DOW,YEAR,MONTH,CUM_MONTH,WEEKEND.
2) Create calendar events dataframe using pandas default calendar events and merge it with master data.
3) Create relative calendar events dataframe like day before event and day after event and merge it to master data.
4) Create double star days(offer days) dataframe and merge it with master data.

Modeling
1) Using the information from forecast_selections, we forecast and save our training model for each item to the appropriate pickle.
2) If sales count > 10,we chose ENSEMBLE MODEL
3) ENSEMBLE model comprises of ARMA,AVERAGE,FB_PROPHET,HOT_WINTER models
4) Filter regression fit variables from list of columns of master data.
    
    Steps involved in ARMA modelling
    --------------------------------------------
    a) create regression fit table with regression_fit_variables as column names.
    b) create linear regression model with input as regression fit table.
    c) predict fitted values using above linear regression model for whole data.
    d) Now pass the residuals from above linear regression model to ARMA model.
    e) Tune ARMA model to get best ARMA order(Check for unit roots, if any <1 then reject and move to next set of params)
    f) Finally get the fitted values from ARMA model and add it to linear regression model fitted values to get final forecast fit.
    
5) HoltWintersAdditiveSeasonality ,fb_prophet modelling both involves training on full data and fitted values will be final forecast fit.
6) Average model forecast value will be average of last 30 days sales.
7) Create stacking ensemble forecast model

    Steps involved in stacking ensemble model building
    --------------------------------------------------
    a) Create the regression prediction table to be used for the meta regression in the ensemble model.
    Example :   ARMA,HW_additive_seasoning,fb_prophet,Average,Adjusted_Sales
                10,11,10.5,12,11.3
                
                f(X) = Y
                ARMA,HW_additive_seasoning,fb_prophet,Average ==> X
                Adjusted_Sales ==> Y
    
    b) Train regression prediction table using linear regression and predict forecast fit values and save the model as pickles.

Masterrun
------------------
1) Load store lift,item lift, promo data
2) Create the regression predict table given the predict variables from forecast start date to forecast end date i.e 21 days.
3) First, create the prediction values from each individual base learner.
4) ARMA model prediction steps : Predict 21 days sales forecast using linear regression model name it as regression_fit and now predict 21 days residuals using arma model name it as arma_residuals,finally add arma_residuals and regression_fit to get final_forecast_fit.
5) Similarily predict forecast fit using hw_additive_seasonality,fb_prophet,average_model.
6) Combine all base learner models results and create a new table with all results of base learners and predict the final forecast using ensemble model by loading the ensmeble model pickle saved in prerun.
7) In the last step calibrate forecasted sales using item lift,store lift,promo lift if applicable and save the results.
    
