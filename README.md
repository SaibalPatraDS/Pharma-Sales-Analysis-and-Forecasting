# Pharma-Sales-Analysis-and-Forecasting

## Introduction

<p>On a larger scale, the sales forecasting in pharmaceutical industry is typically done by using Naïve model, where the forecasted values equal values in the previous period with added factor of growth, which is specifically defined for different regions, markets, categories of products, etc. Although this model fails when the market saturates, in general and on a larger scale, it has proven as successful. Still, analysis and forecasts on a smaller scale, such as single distributor, pharmacy chain or even individual pharmacy, smaller periods such as weeks, etc., guide very important decisions related to resource and procurement planning, what-if analyses, return-on-investment forecasting, business planning and others. The main problem in smaller scale time series analyses and forecasts are significant uncertainties and sales performance very close to random, making the forecasts with accuracies above thresholds as defined by Naïve methods difficult to achieve.
The main research question we tackle is related to exploring the feasibility of use of modern time-series forecasting methods in pharmaceutical products sales forecasting on a smaller scale. In specific, we benchmark the accuracies achieved with those methods against the performances of basic Naïve, Seasonal Naïve and Average methods.
Research work behind the case study considers 8 time series with different statistical features. Each of the time-series summarizes sales of a group of pharmaceutical products. Time-series data are collected from the Point-of-Sale system of a single pharmacy in period of 6 years.
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


## Time Series Forecasting
	
There are many different methods and approaches to time-series forecasting. The simplest methods which are typically used for determining baseline forecasting performance are Average, Naïve and Seasonal Naïve (those models are often used as benchmark models). In Average method, the forecasts of all future values are equal to mean of the historical data. If the dataset is split to train and test sets, then Average of training set is used as a forecast. For Naïve forecasts, all forecasts are set to be values of the last observation, or: f<sub>t+1</sub>=o<sub>t</sub>. Naïve forecasts are considered optimal when data follow a random walk and they can be used only in walk-forward or rolling forecasts, not in long-term forecasting. Seasonal Naïve method is useful for time-series that show seasonality effects. Each forecast is set to be equal to the mean of the observed values from the same time in previous seasonal cycles.




Seasonal decomposition can be also used for forecasting, by building the model (by using any other approach) on the time-series data with subtracted residual component, as calculated by any of the decomposition methods (such as STL). Another, quite successful classical forecasting method is Simple Exponential Smoothing (SES), where forecasts are calculated as: o<sub>t+1</sub>=αo<sub>t</sub>+α(1-α)o<sub>t-1</sub>+ α(1-α)<sup>2</sup>o<sub>t-2</sub>+..+ α(1-α)<sup>n</sup>o<sub>t-n</sub>, with: 0<α<1. Here, the forecast is equal to a weighted average of the past observations, where weights decrease exponentially as we go back in time. There are also improved SES methods which consider trends and seasonality.	

	
	
### [ARIMA method](https://www.statsmodels.org/dev/generated/statsmodels.tsa.arima.model.ARIMA.html)
        
ARIMA (Auto-Regressive Integrated Moving Average) models are most commonly used tools for forecasting univariate stationary time-series. Model uses the dependency relationship (correlation) between an observation and some number of lagged observations (AR) in the past. It is integrated (I), namely it uses differencing (see above) to make time-series stationary, within the method. Finally, it uses the dependency between an observation and a residual error from a moving average model applied to lagged observations (MA). Hyperparameters of one ARIMA model are:
- p: lag order - number of observations in prior time steps included in the model. Typically, it is equal to the lag at which PACF cuts off the cone of the confidence interval
- d: differencing degree - number of times that the raw observations are differenced. If the data series is stationary, d=0. If not, it is d>1.
- q: moving average order - size of the moving average window.

Some common rules for choosing initial AR and MA values (p,q), found in the literature are:

- If the PACF of the differenced series displays a sharp cutoff and/or the lag-1 autocorrelation is positive, then non-zero AR factor should be added to the model. The lag at which the PACF cuts off is the indicated value of p.
- If the ACF of the differenced series displays a sharp cutoff and/or the lag-1 autocorrelation is negative, then non-zero MA factor should be added to the model. The lag at which the ACF cuts off is the indicated value of q.

Some common rules for choosing d are:

- Rule 1 : If the series has positive autocorrelations out to a high number of lags, then it probably needs a higher order of differencing.
- Rule 2 : If the lag-1 autocorrelation is zero or negative, or the autocorrelations are all small and random, then the series does not need a higher order of differencing. If the lag-1 autocorrelation is -0.5 or more negative, the series may be overdifferenced. (Robert Nau, Statistical Forecasting)

Method is improved with SARIMA (Seasonal ARIMA) – SARIMAX Python implementation was used. While SARIMA facilitates direct modeling of the seasonal component of time-series (by considering its own lag order, differencing degree, MA order and an actual lag), SARIMAX provides the extension for using exogenous variables and thus enable multivariate time-series forecasting.
Besides p,d,q parameters of ARIMA, SARIMA also considers additional parameters: P,D,Q,m:

- P: Seasonal autoregressive order.
- D: Seasonal difference order.
- Q: Seasonal moving average order.
- m: The number of time steps for a single seasonal period. m of 12 for monthly data suggests a yearly seasonal cycle.

Some rules for defining the initial set of parameters often used in a literature are as follows:
- m is equal to the ACF lag with the highest value (typically at a high lag).
- D=1 if the series has a stable seasonal pattern over time. D=0 if the series has an unstable seasonal pattern over time.
- P≥1 if the ACF is positive at lag S, else P=0.
- Q≥1 if the ACF is negative at lag S, else Q=0.
- Rule of thumb: P+Q≤2        
        

### [PROPHET Method](https://facebook.github.io/prophet/#:~:text=Prophet%20is%20a%20procedure%20for,several%20seasons%20of%20historical%20data.)
	
Prophet is Facebook’s additive regression model, that includes: linear or logistic trend, yearly seasonal component modeled using Fourier series and user-provided list of important holidays. The model facilitates easy customization and reliable forecasts with default configurations. According to the authors, Prophet is successful for forecasting data with strong "human-scale" seasonality (day of week, time of year), reasonable number of missing data and/or outliers, historical trend changes, non-linear trends (with saturation), at least one year of observations, known holidays.

Prophet model is tuned by using the following hyper-parameters (only selected parameters are noted):

- growth. For a linear trending, value should be 'linear'. If not, 'logistic'. In latter case, the cap (maximum value data will reach) and floor (minimum value data will reach) of predictions need to be provided. This is typically determined by domain experts.
- interval_width. the uncertainty interval to produce a confidence interval around the forecast.
- fourier_order. the number of Fourier components each seasonality is composed of.
- n_changepoints. The changepoints parameter is used when the changepoint dates are supplied instead of having Prophet determine them. In practice Prophet should be let to do that alone.
- changepoint_range usually does not have that much of an effect on the performance.
- changepoint_prior_scale, is there to indicate how flexible the changepoints are allowed to be. In other words, how much can the changepoints fit to the data. If high it will be more flexible, but then overfitting is possible.
- seasonality_prior_scale parameter. This parameter will again allow your seasonalities to be more flexible.	
	

### [Neural Networks](https://www.tensorflow.org/api_docs/python/tf/keras/layers/LSTM)

Long-Short Term Memory (LSTM) are a form of Recurrent Neural Networks (RNN) - deep learning architectures that are characterized by the use of LSTM units in hidden layers. Main feature of RNNs is that they allow information to persist, or they can inform the decision on some classification or regression task in the moment t, by using observations (or decisions) at moments t-1, t-2,.., t-n. In this research, three different LSTM architectures were used. Vanilla LSTM is made of a single hidden layer of LSTM units, and an output layer used to make a prediction. Stacked LSTM is architecture with two or more hidden layers of LSTM units stacked one on top of another. In bidirectional LSTM architecture model learns the input sequences both forward and backward.
	
	
## Methodology 
	
The methodology for implementing this case study follows the typical time series forecasting pipeline, consisting of three major phases: 
1. feature engineering and data preparation; 
2. exploratory data analysis (time-series analysis); and 
3. forecasting.

Based on the problem and objective formal definition, the data acquired from the sales information system are cleaned, feature engineering approach was defined, and all data are transformed to hourly time series, consisting of aggregate sales among different classes of pharmaceutical products in hourly time periods, namely: anti-inflammatory and antirheumatic products (M01AB, M01AE), analgesics and antipyretics (N02BA, N02BE), psycholeptics drugs (N05B, N05C), drugs for obstructive airway diseases (R03) and antihistamines for systemic use (R06). This, intermediary series is used for the formal definition of anomalies and their identification. Also, outliers are detected in consultation with pharmacy staff and treated by first, imputing the missing data and then, by imputing representative data, by using several methods. Finally, data is then rescaled to weekly time-series and stored.

Time series analysis had two-fold objective. First, annual, weekly and daily data analysis were done with objective to make potentially useful conclusions and propositions for improving sales and marketing strategies. Then, stationarity, autocorrelation and predictability analysis of the time series in individual groups was analyzed to infer the initial set of parameters for implementing the forecasting methods.

Forecasting was carried out at the weekly scale. Two different approaches to forecasting problem scoping were adopted. First one implements so called rolling forecast, namely forecasting the sales in the next week, by using the model trained with all historical data. Therefore, during testing, prediction in a timestep t is based on the model which fits the training set consisting of observations in timesteps (0,t-1), or: f(t) = f(o[0:t-1]). Rolling forecast model can be used for short-term resource planning and planning the procurement of stock of pharmaceutical products. Another approach is related to long-term forecasting, for example forecasting the future period of one year, by using the model trained with historical data. This model can be used for business planning and making decisions of strategic nature.

Train-test split validation with one last year of data (52 rows) was used for testing. Key performance indicator for forecasting accuracies in both approaches was Mean Squared Error (MSE). Mean Absolute Percentage Error (MAPE) was provided only as an illustration because data on different groups of pharmaceutical products were on significantly diverse scales. Baseline accuracy was calculated by using Naïve and Seasonal Naïve, for rolling forecasts and Average method for long-term ones. Three different models were tested: ARIMA/SARIMA (for rolling and long-term forecast), Facebook’s Prophet (for rolling and long-term forecast) and Long-Short Term Memory (LSTM) artificial neural network architectures (for long-term forecast).

Hyper-parameters were optimized by using three approaches: manually – with ACF/PACF plot analysis, Python’s statsmodels function and grid search optimization. Grid search was used as adopted approach for hyper-parameters optimization, for ARIMA and Prophet model. LSTM was applied only for long-term forecasting. The data preparation process for LSTM included transforming to stationary time series, sequencing time series to supervised problem data shape [X<sub>t-n_steps</sub>...X<sub>t-2</sub>,X<sub>t-1</sub>][y<sub>t</sub>] (after determining input vector dimension which gives best accuracies), and time series scaling (normalization or standardization). Three LSTM architectures were tested: Vanilla LSTM, Stacked LSTM and Bidirectional LSTM. No optimization of hyper-parameters was carried out. In order to get comparable results, pseudo-random generators of Python, Numpy and Tensorflow were set to fixed values.
	
	
## Feature Engineering 

Initial dataset consisted of 600000 transactional data collected in 6 years (period 2014-2019), indicating date and time of sale, pharmaceutical drug brand name and sold quantity. As a result of the interviews with pharmacists, decision was made that the subject of analyses and forecasting will be actual drug categories, instead of the individual drugs. Thus, selected group of drugs (57 drugs) is classified to 8 Anatomical Therapeutic Chemical (ATC) Classification System categories: 
- M01AB - Anti-inflammatory and antirheumatic products, non-steroids, Acetic acid derivatives and related substances
- M01AE - Anti-inflammatory and antirheumatic products, non-steroids, Propionic acid derivatives
- N02BA - Other analgesics and antipyretics, Salicylic acid and derivatives
- N02BE/B - Other analgesics and antipyretics, Pyrazolones and Anilides
- N05B - Psycholeptics drugs, Anxiolytic drugs
- N05C - Psycholeptics drugs, Hypnotics and sedatives drugs
- R03 - Drugs for obstructive airway diseases
- R06 - Antihistamines for systemic use

ATC codes features are added to the dataset, namely a model has been transformed as indicated on the image below and data was resampled to the hourly time-series.

<img class="img-fluid" src="https://novafabrika.com/notebooks/pharma/datamapping.jpg">	
	
## Conclusion
	
To conclude, time-series analyses and forecasts have guided potentially useful conclusions and recommendations to the pharmacy. Daily, weekly and annual seasonality analysis were proven useful for identifying the periods in which special sales and marketing campaigns could be implemented, except for N05B and N05C categories of drugs which did not exhibit significant regularities. Forecasts have proven better than Naïve methods and in acceptable intervals for long-term planning. It is highly likely that the forecasts could be significantly improved by expanding the problem scope to multivariate time series forecasting and by including explanatory variables, such as:
- Weather data. Sales of antirheumatic drugs in M01AB and M01AE categories could be affected by the changes of atmospheric pressure. Sudden declines in all categories could be explained by extreme weather conditions, such as heavy rain, thunderstorms and blizzards.
- Price of the drugs. Sales spikes may be explained by the discounts, applied in a short term. Introducing this feature may facilitate what-if forecasting analysis of sales performance during marketing campaigns involving price reductions.
- Dates of the pension payoff. Sales spikes are visible at the dates of state pensions payoff.
- National holidays, as non-working days with seasonal patterns similar to Sundays are expected to disrupt daily sales.	
	
	
	
	
	
	
	
	
	
	
	
	
