# Passenger Forecasting Report

**Author:** Nikhil Aji <br>
**Date:** January 2026

# Executive Summary

This project aimed to forecast monthly aviation passenger counts using the data from Statistics Canada spanning 44 years. 

EDA revealed a strong upward trend, annual seasonality, and a sharp decline during the COVID-19 pandemic which is followed by recovery. A Seasonal Autoregressive Integrated Moving Average (SARIMA) model was selected due to the presence of trend and seasonality.

After making the data stationary and performing seasonal differencing, the final model selected was:

SARIMA (1,0,1)(1,1,0,12)

The model achieved an MAPE of ~3.1 and RMSE of ~319,817 passengers. These results indicate strong accuracy. 

The future forecast from October 2025 to October 2027 shows a continued upward trend with seasonal fluctuations. 

# About the Dataset

The dataset is published by **Statistics Canada**. The dataset originally contained monthly data of Operating and financial statistics for major Canadian airlines, which included 4634 rows and 15 columns. Since the intention of this project is to forecast passengers from November 2025 to October 2027, all columns were dropped except the passenger data and date. 

# Introduction

Aviation is an industry where safety and efficiency comes first. By using predictive analytics, air navigation service providers and airlines can identify potential measures that need to be taken. In this project **Seasonal Autoregressive Integrated Moving Average (SARIMA)** model is used to train and forecast passengers using historical **(44 years)** data provided by Statistics Canada. 

# Methodology

## Data Preparation

The data is cleaned to ensure that only necessary data are included for the analysis. From the Operational and financial statistics column passenger counts are selected, and in the values section the passengers count are multiplied by 1000 since the scalar factor of passengers were in thousands. 

The reference date was transformed into pandas datetime format and the data was ordered chronologically. The dataset was indexed by date.

After ensuring all changes were applied properly, all columns were dropped except the reference date and passengers column. 

# Analysis

## Trend
Passenger data was plotted (Fig.1) to understand the dataset further and to visualize the trend. The figure illustrates a clear and strong upward trend. Passenger count gradually increased from around **2 million** in 1980 to around **8 million** by 2020.

## Seasonality
The series shows a clear seasonality trend. Passenger counts peak during summer and drops during winter, which is a common trend in air traffic since most people usually go for vacation during summer months. 

## COVID-19
From early 2020 the trend dropped sharply to ~0 during the global pandemic **COVID-19**.

## Recovery

From 2021 onward the graph shows a strong recovery with passenger volumes returning close to nearly pre-pandemic range. 

![Passenger Trend](/plots/passenger_trend.png)
<p align="center"> Fig.1 Passenger Trend </p>

## Choosing the model

From *Fig. 1* it is clear that the data exhibits strong seasonality, and the passenger volume shows a strong and gradual upward trend. The amplitude of the seasonal fluctuations increases as the trend increases, which indicates that a **Multiplicative decomposition** is appropriate. 

Seasonal decomposition is used to further understand the data, it divides the series into three components: **Trend**, **Seasonality**, and **Residual**. 

Fig. 2 illustrates all three components. Trend shows the overall growth in passenger volume over time after smoothing out the seasonality, the seasonality plot shows the repeating yearly pattern, and the residual plot shows what is left after removing the trend and seasonality

![Seasonal Decompose](/plots/seasonal_decompose.png)

<p align="center"> Fig. 2 Seasonal decompose plot </p>

## Augmented Dickey-fuller (ADF) test

ADF test is used to check whether the time series is stationary or non-stationary.

The test produced an ADF statistic of -1.63 and a p-value of 0.47. Since the p-value is greater than 0.05, it fails to reject the null hypothesis, which indicates the series is non-stationary.

## Seasonal Differencing

Seasonal differencing with a lag of **12** is used to remove the repeating annual pattern, helping to achieve **stationarity**. 

After differencing, the ADF test produced an ADF statistic of -5.9 and a p-value **< 0.001**, which is much smaller than **0.05** (significance level), meaning the series is now **stationary**

Since differencing was done only once and was sufficient to remove non-stationarity, **the value of the order of seasonal differencing (D) is 1** and since non-seasonal differencing is not applied: **d = 0**

## Autocorrelation Function (ACF) and Partial Autocorrelation Function (PCF)

The ACF and PACF plots are used to identify appropriate SARIMA parameters

The ACF plot shows a significant spike at lag 1 and decays gradually, which indicates a non-seasonal moving average component of order one `'q = 1'`. Since there was no significant spikes were observed at lag 12 which suggests that a seasonal moving average `(Q)` terms were not required `(Q=0)`.

The PACF plot shows a strong spike at lag 1 with many lags falling inside the confidence interval, indicating an autoregressive component of order one `(p = 1)`. Since the series shows a strong annual seasonality and after checking the improvement in model statistics , a seasonal autoregressive term of order one (P=1) is selected. 

To confirm P=1, models with and without seasonal autoregressive term (P=1 and P=0) were evaluated. When P=0 is used the `Akaike information criterion (AIC) was 14316.188` and the `MAPE was ~4.6%`. When P=1 is used the `Akaike information criterion (AIC) was 14226.340` and the `MAPE was ~3.1%`.

Based on the improved model fit and forecasting accuracy, `P = 1` is selected for the final SARIMA model.

![ACF_PCF_plots](/plots/ACF_PCF_plot.png) 

<p align="center"> fig.3 ACF and PACF plots </p>

# Forecasting (using the test and train data)

To validate the performance and accuracy of the model the data set is divided into two parts `test` and `train`. The last `24 months` of data were used as the test data and the rest were used to train the model. This allowed the model performance to be evaluated over a two-year forecasting horizon. 

Based on the results a `Seasonal Autoregressive Integrated Moving Average (SARIMA)` model was fitted to the training data. The parameters were `Order  = (1, 0, 1), Seasonal_order = (1, 1, 0, 12)`. After fitting the model forecasts were generated for the next 24 months which corresponds to the test period. 95% confidence interval were also calculated to quantify the uncertainty associated with the forecasts, which provides upper and lower bounds around the predicted values. The forecasts returned a `Mean Absolute Percentage Error (MAPE) of approximately 3.1`. This means the forecasts are close to the actual values. In addition to MAPE, the `Root Mean Square Error (RMSE)` was calculated to measure the average magnitude of forecast errors in absolute terms. The model achieved an RMSE of approximately `319,817 passengers`.

![train_forecast](/plots/train_forecast.png)

<p align="center"> fig.4 Forecasting using training and test data. </p>

# Future Forecast

To predict the future passenger forecast the selected `SARIMA(1,0,1)(1,1,0,12)` model was fitted using the complete dataset. The produced forecasts were for a 24-month period (from October 2025 to October 2027) beyond the available data. The predicted mean represents the expected monthly passenger volumes. 

The forecast indicates a continuation of the historical upward trend in passenger volumes along with the seasonal fluctuations. 
![future_forecast](/plots/future_forecast_plot.png)

<p align="center"> fig.5 Future Forecast. </p>

# Results

The SARIMA model demonstrated strong forecasting performance with an `MAPE of ~3.1%` and an `RMSE of ~319,817 passengers`. Visual comparison of the actual and predicted values shows that the model closely follows the observed trends and seasonal pattern. 

# Conclusion

The SARIMA model captured the trend and seasonality in Canadian passenger data. With a MAPE of ~3.1% and low RMSE, the model provides reliable short-term forecasts and indicates continued seasonal growth in passenger volumes.


# <u>References </u>

* Government of Canada, S. C. (2023, February 23). Operating and financial statistics for major Canadian airlines, monthly. Www150.Statcan.gc.ca. https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=2310007901

* GeeksforGeeks. (2025, June 17). Augmented DickeyFuller (ADF). GeeksforGeeks. https://www.geeksforgeeks.org/machine-learning/augmented-dickey-fuller-adf/

* LEONIE. (2022). Time Series: Interpreting ACF and PACF. Kaggle.com. https://www.kaggle.com/code/iamleonie/time-series-interpreting-acf-and-pacf

* gauravduttakiit. (2020, November 12). Forecasting with SARIMA Method. Kaggle.com; Kaggle. https://www.kaggle.com/code/gauravduttakiit/forecasting-with-sarima-method

* Roberts, A. (2023, February 2). Mean Absolute Percentage Error (MAPE): What You Need To Know. Arize AI. https://arize.com/blog-course/mean-absolute-percentage-error-mape-what-you-need-to-know/

* Olumide, S. (2023, August 8). Root Mean Square Error (RMSE): What You Need To Know. Arize AI. https://arize.com/blog-course/root-mean-square-error-rmse-what-you-need-to-know/

* What is the difference between additive and multiplicative time series models? (2025). Milvus.io. https://milvus.io/ai-quick-reference/what-is-the-difference-between-additive-and-multiplicative-time-series-models

* Lewinson, E. (2022, March 31). Time Series DIY: Seasonal Decomposition | Towards Data Science. Towards Data Science. https://towardsdatascience.com/time-series-diy-seasonal-decomposition-f0b469afed44/

* GeeksforGeeks. (2025, June 17). Augmented DickeyFuller (ADF). GeeksforGeeks. https://www.geeksforgeeks.org/machine-learning/augmented-dickey-fuller-adf/

* Monigatti, L. (2023, April 11). Stationarity in Time Series - A Comprehensive Guide | Towards Data Science. Towards Data Science. https://towardsdatascience.com/stationarity-in-time-series-a-comprehensive-guide-8beabe20d68/

* GeeksforGeeks. (2023, November 17). SARIMA (Seasonal Autoregressive Integrated Moving Average). GeeksforGeeks. https://www.geeksforgeeks.org/machine-learning/sarima-seasonal-autoregressive-integrated-moving-average/