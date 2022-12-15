# Pharma-Sales-Analysis-and-Forecasting

## Introduction

<p>On a larger scale, the sales forecasting in pharmaceutical industry is typically done by using Naïve model, where the forecasted values equal values in the previous period with added factor of growth, which is specifically defined for different regions, markets, categories of products, etc. Although this model fails when the market saturates, in general and on a larger scale, it has proven as successful. Still, analysis and forecasts on a smaller scale, such as single distributor, pharmacy chain or even individual pharmacy, smaller periods such as weeks, etc., guide very important decisions related to resource and procurement planning, what-if analyses, return-on-investment forecasting, business planning and others. The main problem in smaller scale time series analyses and forecasts are significant uncertainties and sales performance very close to random, making the forecasts with accuracies above thresholds as defined by Naïve methods difficult to achieve.
The main research question we tackle is related to exploring the feasibility of use of modern time-series forecasting methods in pharmaceutical products sales forecasting on a smaller scale. In specific, we benchmark the accuracies achieved with those methods against the performances of basic Naïve, Seasonal Naïve and Average methods.
Research work behind the paper considers 8 time series with different statistical features. Each of the time-series summarizes sales of a group of pharmaceutical products. Time-series data are collected from the Point-of-Sale system of a single pharmacy in period of 6 years.
This paper is structured into 4 main parts. First, short theoretical background for time series analysis and forecasting is provided to inform the reader on the credibility of decisions made in the implementation of this case study. Then, research methodology, actually a problem-neutral time series forecasting pipeline is presented. Next, the actual implementation is presented, by highlighting the steps made in following the proposed methodology in the case of pharmaceutical products sales data analysis and forecasting. Finally, the discussion brings the description of actual results and some suggestions to the sales department, driven by the result of the data analysis.<p>


## Theoritical Background
	
Time series is a sequence of observations recorded at regular time intervals (hourly, daily, weekly, monthly, quarterly and yearly). Its analysis involves understanding various aspects of the time series, important for creating meaningful and accurate forecasts.

Typically, a time-series data embodies each of the four different components:

level (mean of time-series data);
trend (long-term increasing or decreasing value in series);
seasonality (the repeating short-term cycle in the series); and
noise or residuals (random variations in the series).
One time-series can be assumed to be additive or multiplicative (although, in a real world, series that fit one or another model rarely exist). For additive series, time-dependent variable value is equal to the addition of four components, namely,

y(t) = Level + Trend + Seasonality + Noise

In multiplicative series, changes increase or decrease over time:

y(t) = Level * Trend * Seasonality * Noise

Each time-series can be decomposed to its four components, by using different methods, such as Naïve, Loess or STL. Decomposition is especially useful for trend and uncertainty (variance of residuals typically corresponds to randomness) analysis.

Stationarity of time-series is the property of exhibiting constant statistical properties over time (for example, mean, variance, autocorrelation). It can be visually determined by plotting rolling statistics (rolling means and variances) or by using Augmented Dickey-Fuller (ADF) or Kwiatkowski-Phillips-Schmidt-Shin (KPSS) test. ADF test assumes that the null hypothesis is the time series possesses unit root and is non-stationary. If the P-Value of ADF test is less than 0.05, the null hypothesis is rejected and series is considered as stationary. KPSS is used to test for trend stationarity. One series is considered trend stationary if after removing the underlying trend, series becomes stationary.

In case that time-series is non-stationary, it needs to be transformed. Following transforms for stationarizing data (for methods which do not work well with non-stationary data, for example, ARIMA) are available: 1) de-trending (removing the underlying trend in the series), 2) differencing (seasonal or cyclic patterns are removed by subtracting periodical values), 3) logging (linearizing trend with exponential function).

Basic time-series forecasting assumes regressing the observation at the time t on the observations in the previous time steps: f<sub>t</sub>=e<sub>1</sub>o<sub>t-1</sub>+ e<sub>2</sub>o<sub>t-2</sub>+…+e<sub>n</sub>o<sub>t-n</sub>. The potential strength of time-series prediction models is determined by autocorrelation – correlation between what is considered as output variable Ot and input variables – prior observations. Autocorrelation plots graphically summarize the strength of a relationship of an observation in a time series with observations at prior time steps, with respect to the confidence interval (typically, 95% of confidence). Typically, Pearson coefficient of correlation is used, meaning that linear correlation is assessed. These plots are often called Autocorrelation Function (ACF) plots or correlograms. Another useful tool for determining autocorrelation are Partial Autocorrelation Function (PACF) plots. PACF plots highlight autocorrelation between observation ot and prior observations oi, without taking into account correlations in the time steps in the interval (t,i).

Approximate or Sample Entropy methods can be used to quantify the regularity and predictability of fluctuations in a time series. The higher the entropies are, the more difficult is to forecast the time series. As difficulty increases, the time series converge to what is called – white noise, series of random numbers with mean equals 0.


