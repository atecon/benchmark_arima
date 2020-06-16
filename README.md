# benchmark_arima
Comparison of estimation speed of ARIMA-type models between Python's 'statsmodels' module and Gretl's built-in aparatus.

*Key findings*
- Both Gretl's built-in aparatus of the Kalman filter and the optimization algorithm (BFGS) is very fast compared to Python and 'statsmodels'.
- We find that -- dependend on the ARIMA parameter settings -- gretl is about 100 times fast. For some settings even much faster.
- Surprisingly, even for simple AR(1) models gretl is about 50 times faster.

# Problem
ARIMA type of time-series models are widely applied. Speed of estimation the model's parameters is crucial.

For instance, if one wants to estimate separate models for a sales forecasting probleme for hundred thousands of articles, computational time is relevant. Also, the construction of probability forecasts or Monte Carlo simulations are computationally heavy.

This project compares the estimation speed of 'statsmodels', a wll-known popular Python library, with gretl's built-in ```arima``` command.

More on gretl here: http://gretl.sourceforge.net/
Details on gretl's arima command: http://gretl.sourceforge.net/gretl-help/cmdref.html#arima

# Benchmark settings and results
## Benchmark settings
Using an hourly data set of electricity loads and temperature dynamics (T = 744). The data set is stored in "./data/electricity".

We estimate several SARIMAX type of models:
*Endogenous*: ```load```
*Exogenous*: ```temp``` and ```intercept```

The exercise is done for the followong parameter ranges:

- ```p```: [1]
- ```d```: [0]
- ```q```: [0, 1]
- ```P```: [0, 1]
- ```D```: [0]
- ```Q```: [0, 1]

The exercise is also conducted for different sample lenghts:
- ```T```: [50, 150, 500, 744]


*NOTE*: We've evidence that results look similarly 'bad' for Python for much larger SARIMAX types of models with 15 exogenous variables

## Measuring differences
For all parameter combinations we compute the ratio of speed as time(python) / time(gretl).

- A ```ratio``` of one indicates the same speed.
- If ```ratio``` > 1 Python is x percent lower compared to gretl.
- If ```ratio``` < 1 Python is x percent faster compared to gretl.


## Versions used
- Gretl: 2020c (build data 2020-06-16)
- Python: Python 3.8.1
- statsmodels: 0.11.0


## Some details on the underlying algorithms
TBA


# Replication files
Python code: <./script/run_python.inp> includes the actual Python code in gretl's "foreign block" which allows to execute Python from Gretl. The Python job also writes the <./data/python_arima_duration.csv>.
Simply copy and paste the Python code for running it outside Gretl.

Gretl code: <./script/run.inp> includes the Gretl code for running the exercise using Gretl. It also compares the results of Gretl and Python (reading <./data/python_arima_duration.csv>), and write thes file <summary_results.txt>.


# Results (<summary_results.txt>)
	Summary speed comparison estimating ARIMA-type models.
	between Python's 'statsmodels' package and gretl's built-in aparatus.

	Dataset used: ./data/electricity

	Legend:
	k: Number of exogenous variables (incl. intercept)
	ratio: time(Python) / time(gretl)
	T: sample length
	p, d, q, P, D, Q: respective SARIMA parameters


         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000    71.612    50.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   252.409    50.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000    70.170    50.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000    13.363    50.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000   389.026    50.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   251.331    50.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   152.028    50.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000       nan    50.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000   214.530   150.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   266.426   150.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   392.847   150.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000    44.258   150.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000   169.700   150.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   154.813   150.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   200.396   150.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000    94.890   150.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000    46.485   500.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   263.114   500.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000  1072.107   500.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   156.242   500.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000    71.163   500.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   170.018   500.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   420.875   500.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   119.827   500.000

         k         p         d         q         P         D         Q     ratio         T
     2.000     1.000     0.000     0.000     0.000     0.000     0.000    52.148   744.000
     2.000     1.000     0.000     0.000     0.000     0.000     1.000   154.960   744.000
     2.000     1.000     0.000     0.000     1.000     0.000     0.000   475.544   744.000
     2.000     1.000     0.000     0.000     1.000     0.000     1.000   191.071   744.000
     2.000     1.000     0.000     1.000     0.000     0.000     0.000   115.788   744.000
     2.000     1.000     0.000     1.000     0.000     0.000     1.000   274.934   744.000
     2.000     1.000     0.000     1.000     1.000     0.000     0.000   765.818   744.000
     2.000     1.000     0.000     1.000     1.000     0.000     1.000   147.624   744.000

