# benchmark_arima
Comparison of estimation speed of ARIMA-type models between Python's 'statsmodels' module and Gretl's built-in apparatus.

*Key findings*
- Both Gretl's built-in apparatus of the Kalman filter and the optimization algorithm (BFGS) is very fast compared to Python and 'statsmodels'.
- We find that -- dependent on the ARIMA parameter settings -- Gretl is about 100 times faster. For some settings even much faster.
- For *none* of the cases Python/ statsmodels dominates.
- Surprisingly, even for simple AR(1) models gretl is at least 60 times faster.

# Problem
ARIMA type of time-series models are widely applied. Speed of estimation the model's parameters is crucial.

For instance, if one wants to estimate separate models for a sales forecasting probleme for hundred thousands of articles, computational time is relevant. Also, the construction of probability forecasts or Monte Carlo simulations are computationally heavy.

This project compares the estimation speed of 'statsmodels', a well-known popular Python library (https://www.statsmodels.org/stable/index.html), with gretl's built-in ```arima``` command.

More on gretl here: http://gretl.sourceforge.net/

Details on gretl's arima command: http://gretl.sourceforge.net/gretl-help/cmdref.html#arima

# Benchmark settings and results
## Benchmark settings
An hourly data set of electricity loads and temperature dynamics (T = 744) is used. The data set is stored in "./data/electricity".

We estimate several SARIMAX type of models:
- Endogenous: ```load```
- Exogenous: ```temp``` and ```intercept```

The exercise is done for the following parameter ranges:

- ```p```: [1]
- ```d```: [0]
- ```q```: [0, 1]
- ```P```: [0, 1]
- ```D```: [0]
- ```Q```: [0, 1]

The exercise is also conducted for different sample lengths:
- ```T```: [50, 150, 500, 744]


*NOTE*: We've evidence that results look similarly 'bad' for Python for much larger SARIMAX types of models with 15 exogenous variables

## Measuring differences
We compare the execution time of the following commands only:

Gretl:

    arima p d q ; P D Q ; y L

Python:

    model = ARIMA(y, exog=X, order=(p, d, q), seasonal_order=(P, D, Q, 24), trend='c')
    model_fit = model.fit()


For all parameter combinations we compute the ratio of speed as: ```time(python) / time(gretl)```.

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
Python code: <./script/run_python.inp> includes the actual Python code in gretl's "foreign block" which allows to execute Python from Gretl. The Python job also writes the file <./data/python_arima_duration.csv>.
Simply copy and paste the Python code for running it outside Gretl.

Gretl code: <./script/run_gretl.inp> includes the Gretl code for running. Details of the computational time as well as of the number of both function and gradient evaluations are stored in <./output/gretl_results.txt>
It also compares the results of Gretl and Python (reading <./data/python_arima_duration.csv>), and writes the file <./output/comparison_results.txt>.


# Results (<./output/comparison_results.txt>)
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

