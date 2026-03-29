# BlackRock (BLK) Stock Price Forecasting: A 10-Year Time Series Analysis

## About the Project
This project involves the development of quantitative forecasting models to analyze and predict the stock price trajectory of BlackRock Inc. (NYSE: BLK). By conducting a comprehensive 10-year longitudinal analysis of historical market data, the project aims to identify underlying structural components, such as deterministic trends and subtle seasonality that drive asset valuation. The end goal is to provide a robust time series framework that compares deterministic, smoothing, and probabilistic algorithms to accurately forecast future stock prices and evaluate market risk.

## Team Members
* [Janna Freund](https://github.com/jannafr) (ETU20250782)
* [Tetiana Fedotova](https://github.com/mesopotania) (ETU20250781)
* [Mridul Jain](https://github.com/MridulJain1101) (ETU20250784)
* [Kezia Wibowo](https://github.com/kezia-ch) (ETU20240232)

## About the Dataset
The financial data utilized for this project was sourced directly from **Yahoo Finance** using the `quantmod` package in R. 
* **Asset:** BlackRock Inc. (Ticker: BLK)
* **Timeframe:** March 1, 2016 – February 28, 2026 (10 Years)
* **Target Variable:** The `Adjusted Close` price was selected as the primary metric because it accurately reflects the asset's true value after accounting for corporate actions, such as dividend payouts and stock splits.

## Analysis Approach & Methodology
The analysis was conducted in a structured, step-by-step pipeline, transitioning from raw daily market data to advanced predictive modeling:

### 1. Data Transformation & Aggregation
The raw daily stock data was aggregated into a monthly frequency. This transformation reduced microscopic market noise and high-frequency volatility, thereby enhancing the stability and reliability of the time series forecasting models.

### 2. Visual Diagnostics & Seasonality Check
Initial visual inspection of the time series revealed a pronounced deterministic upward trend over the 10-year period. By isolating the first 36 months of data, we observed no strict, recurring seasonal patterns, establishing a baseline understanding of the asset's structural behavior.

### 3. Baseline Modeling: Moving Averages & Linear Regression
The dataset was split into training (up to Feb 2024) and testing (March 2024 onwards) sets. We established baseline forecasts using Simple Moving Averages (3, 6, and 12 months) and a Linear Trend regression model. While the Linear Regression confirmed a statistically significant trend of ~$5.17 USD/month ($R^2 = 0.74$), it proved too rigid to capture non-linear price accelerations.

### 4. Exponential Smoothing Techniques
We constructed competing hypotheses regarding the underlying structural components of the stock price and applied multiple smoothing techniques:
* Simple Exponential Smoothing (SES)
* Holt’s Linear Trend
* Additive Holt-Winters
* Multiplicative Holt-Winters (MULT_HW)

### 5. Stationarity Diagnostics (KPSS Test) & Differencing
To formally test for stationarity, we applied the Kwiatkowski-Phillips-Schmidt-Shin (KPSS) test. The initial test statistic ($2.1444$) indicated a non-stationary series. We subsequently applied a logarithmic transformation (to stabilize variance) and a first-order difference (to eliminate the trend), successfully achieving strict stationarity ($0.0488$).

### 6. Probabilistic Modeling: ARIMA
Using the transformed data and Auto-Correlation/Partial Auto-Correlation Functions (ACF/PACF), we utilized the `auto.arima()` algorithm. It identified an `ARIMA(1,0,0)[12]` model with drift. However, statistical significance testing revealed that the autoregressive parameters were weak, meaning the model behaved essentially as a random walk.

### 7. Model Evaluation & Selection
All models were evaluated against the out-of-sample test data using the Mean Absolute Percentage Error (MAPE). MAPE was selected as the primary metric because its scale-independence ensures a fair comparison across a 10-year period during which BLK's price tripled.

## Conclusions
The comparative modeling approach demonstrated that the **Multiplicative Holt-Winters (MULT_HW)** model achieved the lowest MAPE on the test data. This indicates that BlackRock's stock price trajectory is best defined by an ongoing structural upward trend compounded by *proportional cyclical fluctuations* (where price volatility grows proportionally with the stock's level). 

Conversely, the ARIMA model underperformed because it essentially defaulted to a random walk ("tomorrow will look like today"), failing to generalize the long-term upward trend. Based on our optimal model's predictions, the forecasted market price for BLK maintains a bullish trajectory, though investors must account for the expanding prediction intervals which represent increasing market uncertainty over a longer time horizon.

## Tools and Techniques
* **Language:** R
* **Financial Data Extraction:** `quantmod`
* **Data Manipulation:** `dplyr`, `lubridate`
* **Time Series Forecasting:** `forecast`, `TTR`
* **Statistical Diagnostics:** `urca` (KPSS testing), `lmtest`
* **Evaluation Metrics:** `Metrics` (MAPE, RMSE, MAE)
* **Data Visualization:** `ggplot2`
