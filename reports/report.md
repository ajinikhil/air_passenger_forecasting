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

# Results

# Conclusion
