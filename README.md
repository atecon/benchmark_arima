# benchmark_arima
Comparison of estimation speed of ARIMA-type models between Python's 'statsmodels' module and Gretl's built-in aparatus.

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


# Results
{{summary_results.txt}}




