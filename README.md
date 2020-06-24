# SARIMAX estimation speed comparison between Python's 'statsmodels' package and Gretl
The well-known M4 Competition on forecasting (https://www.sciencedirect.com/science/article/pii/S0169207019301128) indicated that pure machine learning and neural network methods performed worse than standard algorithms like ARIMA. Thus, even though not well-known in the data science community, yet -- under the tag *ARIMA* you'll find only 3 articles under kdnuggets.com (https://www.kdnuggets.com/tag/arima) -- ARIMA of time-series models belong to the standard repertoire of any data scientist or academic (for a mathemtical elaboration see Lütkepohl (2006)).

The purpose of this project is to compare the speed of estimating the model coefficients of an ARIMA-type model for both Python's popular 'statsmodels' package and Gretl's built-in arima apparatus. Speed of estimation the model's parameters is crucial. For instance, if one wants to estimate separate models for a sales forecasting probleme for hundred thousands of articles, computational time is relevant. Also, the construction of probability forecasts or Monte Carlo simulations are computationally heavy.

In the following we will describe our evaluation setting and use hourly time-series for illustration. We will estimate several different models of the so called SARIMAX type. SARIMAX stands for **seasonal** (S) **autoregressive** (AR) **integrated** (I) **moving-average** (MA) **with exogenous regressors** (X).

## Key findings
- Both Gretl's built-in apparatus for estimating ARIMA-type of models is *very* fast compared to 'statsmodels'.
- We find that -- dependent on the exact ARIMA parameter settings -- Gretl is about 100 times faster. For some settings even much faster.
- For *none* of the cases considered, Python's 'statsmodels' dominates.
- Surprisingly, even for simple AR(1) models gretl is at least 60 times faster.

## References
-  Python's statsmodels: https://www.statsmodels.org
- Gretl: http://gretl.sourceforge.net/ and https://youtu.be/l-xHXE17SiY
- Details on Gretl's arima command: http://gretl.sourceforge.net/gretl-help/cmdref.html#arima

# Benchmark settings and results
## Benchmark settings
An hourly data set of electricity loads and temperature dynamics (T = 744) is used. The data set is stored in in this repo under "./data/electricity".

We estimate several small-scale SARIMAX type of models. The following variables are used:
- Endogenous variable: ```load``` (time-series on electirity consumed)
- Exogenous variable: ```temp``` (temperature) and ```intercept```

The exercise is done for the following parameter ranges:

- ```p```: [1]
- ```d```: [0]
- ```q```: [0, 1]
- ```P```: [0, 1]
- ```D```: [0]
- ```Q```: [0, 1]

We also run the exercise for different sample lengths:
- ```T```: [50, 150, 500, 744]

*NOTE*: We've evidence that results look similarly 'bad' for Python's 'statsmodels' for much larger SARIMAX types of models with 15 exogenous variables

## Measuring differences
We compare the execution time of the following commands only:

Gretl:
    arima p d q ; P D Q ; y L

Python:
    model = ARIMA(y, exog=X, order=(p, d, q), seasonal_order=(P, D, Q, 24), trend='c')
    model_fit = model.fit()


For all parameter combinations we compute the ratio of speed as: ```time(python) / time(gretl)``` (in seconds).
- A ```ratio``` of 1 indicates the same speed.
- If ```ratio``` > 1 Python is x percent lower compared to gretl.
- If ```ratio``` < 1 Python is x percent faster compared to gretl.


## Software versions and system used
- Gretl: 2020c (build data 2020-06-16)
- Python: Python 3.8.1
- statsmodels: 0.11.0
- Ran on Ubuntu 20.04
- Dual core Intel(R) Core(TM) i7-3520M CPU @ 2.90GHz

## Some details on the underlying algorithms
- Gretl makes use of Guy Melard's algorithm AS197 (written in C) instead of using some Kalman filter.
- Python makes use of its own Kalman apparatus instead (using some C library).

# Replication files
Python code: ```./script/run_python.inp``` includes the actual Python code in gretl's "foreign block" which allows to execute Python from Gretl. The Python job also writes the file <./data/python_arima_duration.csv>.
Simply copy and paste the Python code if you would like to run it in Python.

Gretl code: ```./script/run_gretl.inp``` includes the Gretl code for running the exercise. Details of the computational time as well as of the number of both function and gradient evaluations are stored in ```./output/gretl_results.txt```. It also compares the results of Gretl and Python (reading ```./data/python_arima_duration.csv```), and writes the file ```./output/comparison_results.txt```.


# Results (```./output/comparison_results.txt```)
	Summary speed comparison estimating ARIMA-type models.
    between Python's 'statsmodels' package and gretl's built-in apparatus.

    Dataset used: ./data/electricity.csv

    Legend:
    k: Number of exogenous variables (incl. intercept)
    ratio: time(Python) / time(gretl)
    T: sample length
    p, d, q, P, D, Q: respective SARIMA parameters


         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000   104.214    50.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   921.115    50.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   387.487    50.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   604.610    50.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000   356.454    50.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   994.446    50.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   639.574    50.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   597.165    50.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000   254.626   150.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   722.975   150.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   557.668   150.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   532.198   150.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000   129.406   150.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   530.360   150.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   315.756   150.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   613.605   150.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000    62.697   500.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   501.449   500.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   977.901   500.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   283.050   500.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000    62.054   500.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   769.268   500.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   491.848   500.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   295.669   500.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000    63.766   744.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   227.211   744.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   463.396   744.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   337.743   744.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000    75.617   744.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   701.828   744.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   541.396   744.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   360.170   744.000


# References
- Lütkepohl, Helmut (2006): *New Introduction to Multiple Time Series Analysis*, Springer.
- Spyros Makridakis, Evangelos Spiliotis, Vassilios Assimakopoulos (2020): "The M4 Competition: 100,000 time series and 61 forecasting methods", *International Journal of Forecasting*, Volume 36, Issue 1, Pages 54-74, https://doi.org/10.1016/j.ijforecast.2019.04.014.
