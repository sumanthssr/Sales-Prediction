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
    
    Steps involved in ARMA modelling
    a)







Masterrun
------------------
