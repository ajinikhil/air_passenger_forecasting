# Passenger Forecasting Report

**Author:** Nikhil Aji <br>
**Date:** January 2026

# Executive Summary

# About the Dataset

The Dataset is published by **Stastics Canada**. The dataset originally contained monthly data of Operating and financial statistics for major Canadian airlines, which included 4634 rows and 15 columns. since the intention of this project is to forecast passengers from 2025 November to 2027 October, all the columns are dropped except the passenger data and date. 

# Introduction

Aviation is an indrustry where safety and efficiciency comes first. By using predictive analytics air navigation service provider/airlines can identify potential measures that needed to be taken. In this project **Seasonal Autoregressive Integrated Moving Average (SARIMA)** model is used to train and forecast passengers using historical **(44 years)** data provided by stastics canada. 

# Methodology

## Data Preparation

The data is cleaned to ensure that only necessary data are included for the analysis. From the Operational and financial statistics column passenger values are selected, and in the values section the passengers count are multiplied by 1000 since the scalar factor of passengers were in thousands. 

The reference date was transformed into pandas datetime format and the data was ordered chronologically. The dataset was indexed by date.

After making sure all the changes made are applied properly all the columns are dropped except the reference date and passengers column. 

# Analysis

## Trend
Passenger data was plotted (Fig.1) to understand the dataset further and to visualize the trend. The figure illustrates a clear and strong upward trend. Passenger count gradually increased from arounf **2 million** in 1980 to around **8 million** by 2020.

## Seasonality
The series shows a clear seasonality trend. Passenger counts peaks during summer and drops during winter, which is a common trend in air traffic since most people usually go for vaccation dunig summer months. 

## COVID-19
From early 2020 the trend dopped sharply to ~0 during the golbal pandamic **COVID-19**.

## Recovery

From 2021 onward the graph shows a strong recovery with passenger volumes returning close to nearly pre-pandamic range. 

![Passenger Trend](/plots/passenger_trend.png)
<p align="center"> Fig.1 Passenger Trend </p>

## Choosing the model

From *Fig. 1* it is clear that the data exhibits strong seasonality, and the passenger volume shows a strong and gradual upward trend. The amplitude of the seasonal fluctuations increases as the trend increases, which indicates that a **Multiplicative decomposition** is appropriate. 

Seasonal decompose is used to further understand the data, it divides the series into three components: **Trend**, **Seasonality**, and **Residual**. 

Fig. 2 illustrates all three components. Trend shows the overall growth in passenger volume over time after smoothing out the seasonality, the seasonality plot shows the repeating yearly pattern, and the residual plot shows what is left after removing the trend and seasonality

![Seasonal Decompose](/plots/seasonal_decompose.png)

<p align="center"> Fig. 2 Seasonal decompose plot </p>

## Augmented Dickey-fuller (ADF) test

ADF test is used to check whether the time series is stationary or non-stationary.

The test produced an ADF statistics of -1.63 and a p-value of 0.47. Since the p-value is greater than 0.05, it fail to reject the null hypothesis, which indicates the series is non-stationary.

## Seasonal Differencing

Seasonal differencing with a lag of **12** is used to remove the repeating annual pattern, helping to achieve **stationarity**. 

After the differencing the ADF test produced a result, ADF statistics of -5.9 and a p-value  **<0.001**, which is much smaller than **0.05** (significance level), meaning the series is now **stationary**

Since the differencing was done only one and was sufficient enough to remove the non-stationarity, **the value of the oreder if seasonal differencing(D) is 1** and since non-seasonal differencing is not applied: **d = 0**

## Autocorrelation Function (ACF) and Partial Autocorrelation Function (PCF)

The ACf and PACF plots are used to identify appropriate SARIMA parameters

The ACF plot shows a significant spike at lag 1 and decays gradually, which indicates a non-seasonal moving average component of order one `'q = 1'`. Since there was no significant spikes were observed at lag 12 which suggests that a seasonal moving average `(Q)` terms were not required `(Q=0)`.

The PACF plot shows a stong spike at lag 1 with many lags falling inside the confdence interval, indicating an autoregressive component of order one `(p = 1)`. Since the series shows a strong annual seasonality and after checking the improvement in model statestics , a seasonal autoregressive term of order one (P=1) is selected. 

To confirm P=1, models with and without seasonal autoregressive term (P=1 and P=0) were evaluvated. When P=0 is used the `Akaike information criterion (AIC) was 14316.188` and the `MAPE was ~4.6%`. When P=1 is used the `Akaike information criterion (AIC) was 14226.340` and the `MAPE was ~3.1%`.

Based on the improved model fit and forecasting accuracy, `P = 1` is selected for the final SARIMA model.

![ACF_PCF_plots](/plots/ACF_PCF_plot.png)

# Forecasing (using the test and train data)

To validate the perfomance and accuracy of the model the data set is divided into two parts `test` and `train`. The last `24 months` of data were used as the test data and the rest were used to train the model. This allowed the model performance to be evaluvated over a two-year forecasting horizon. 

Based on the results a `Seasonal AutoRegressive Integrated Moving Averaege (SARIMA)` model was fitted to the training data. The parametes were `Order  = (1, 0, 1), Seasonal_order = (1, 1, 0, 12)`. After fitting the model forecasts were generated for the next 24 months which corresponds to the test period. 95% confidence interval were also calculated to quantify the uncertainty associated with the forecasts, which provides upper and lower bounds around the predicted values. The forecasts returned a `Mean Absolute Percentage Error (MAPE) of approximatly 3.1`. Which means the forecasts are close to the actual value. In addition to MAPE, the `Root Mean Square Error (RMSE)` was calculated to measure the average magnitude of forecast errors in absolute terms. The model achieved an RMSE of approximately `319,817 passengers`.

# Future Forecast

To predict the future passenger forecast the sected `SARIMA(1,0,1)(1,1,0,12)` model was fitted using the complete dataset. Produced forecasts were for a 24-month period (from October 2025 to October 2027) beyond the available data. Predicted mean represent the expected monthly passenger volumes. 

The forecast indicate a continuation of the the historical upward trend in passenger volumes along with the seasonal fluctuations. 


# Results

# Conclusion
