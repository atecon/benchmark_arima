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
Using an hourly data set of electricity loads (./data/electricity) and temperature dynamics

## Results
For



## Versions used

- Gretl: 2020c (build data 2020-06-16)
- Python: Python 3.8.1
- statsmodels: 0.11.0


