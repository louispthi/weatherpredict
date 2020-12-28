# Weatherpredict

Weatherpredict is a machine learning project whose goal is to predict various weather data. 

The project was inspired by a series of three articles by Adam McQuistan.

<https://stackabuse.com/using-machine-learning-to-predict-the-weather-part-1/>

The data set is different.

### Data set 

We obtained historical weather data for Montreal from OpenWeatherMap. 
The data contains hourly information from January 1st, 1979 to July 31st, 2020. 

The collected features are: 

- <code> city_name </code> City name
- <code> lat </code> Geographical coordinates of the location (latitude)
- <code> lon </code> Geographical coordinates of the location (longitude)
- <code> main </code>
    - <code> main.temp </code> Temperature
    - <code> main.feels_like </code> This temperature parameter accounts for the human perception of weather
    - <code> main.pressure </code> Atmospheric pressure (on the sea level), hPa
    - <code> main.humidity </code> Humidity, %
    - <code> main.temp_min </code> Minimum temperature at the moment. This is deviation from temperature that is possible for large cities and megalopolises geographically expanded (use these parameter optionally).
    - <code> main.temp_max </code> Maximum temperature at the moment. This is deviation from temperature that is possible for large cities and megalopolises geographically expanded (use these parameter optionally).
- <code> wind </code>
    - <code> wind.speed </code> Wind speed. Unit Default: meter/sec
    - <code> wind.deg </code> Wind direction, degrees (meteorological)
- <code> clouds </code>
    - <code> clouds.all </code> Cloudiness, %
- <code> rain </code>
    - <code> rain.1h </code> Rain volume for the last hour, mm
    - <code> rain.3h </code> Rain volume for the last 3 hours, mm
- <code> snow </code>
    - <code> snow.1h </code> Snow volume for the last hour, mm (in liquid state)
    - <code> snow.3h </code> Snow volume for the last 3 hours, mm (in liquid state)
- <code> weather </code> 
    - <code> weather.id </code> Weather condition id
    - <code> weather.main </code> Group of weather parameters (Rain, Snow, Extreme etc.)
    - <code> weather.description </code> Weather condition within the group
    - <code> weather.icon </code> Weather icon id
- <code> dt </code> Time of data calculation, unix, UTC
- <code> dt_iso </code> Date and time in UTC format
- <code> timezone </code> Shift in seconds from UTC

The explanation for the weather condition id and icon id can be found [here](https://openweathermap.org/weather-conditions).

### Files 

We have the following files. 

* cleaning_datas.ipynb : perform initial cleaning of the data that will be used in our projects. 

<<<<<<< HEAD
## Project 1: Linear Regression model to predict average daily temperatures

The goal is to use linear regression to predict the temperature on a given day using the three previous days.

We resample the hourly data into daily data. We also analyse the different features and check for collinearity and correlation. We only keep features most correlated with our target variable, that is, temperature on previous days, wind speed and snow. We could have combined snow and rain into one single feature 'precipitation' to see if there was any difference.  However, rain did not seem very corelated to temperature. 

We fit a linear regression model, a Ridge regression model and a Lasso regression model. The last two models were used more as educational purposes, as it was not expected that they would improve linear regression, since we had removed collinearity problems in the data cleaning and there was a clear linear trend between our target and feature variables.  

We also add polynomial features to remove possible bias, with limited success. In fact, degree 2 polynomials were performing best, but only slightly. 

### Files 

We have the following files in the lin_reg folder.

* cleaning_datas_lin_reg.ipynb : perform data cleaning to prepare for a linear regression model;
* weather_lin_reg_model.ipynb : create linear regression models and analyse the data. 
	
### Conclusion 

Our models did better than the 'naive model', where we simply predict that the temperature on a given day is the same as the previous day. 

We obtained the following: 

 - Explained Variance: 0.91
 - Mean Absolute Error: 2.55 degrees celsius
 - Median Absolute Error: 1.93 degrees celsius

There was no significant difference between Lasso regression, Ridge regression and linear regression. This is surprising because there is a high correlation between some variables, so we would have expected to see a higher alpha parameter. We will need further analysis.

## Project 2: LSTM model to predict the next 36 hours of temperatures

The goal is to use a LSTM RNN model to predict the temperature over a 36-hour period using the four previous days. From readings, this model seems appropriate for time-series predictions. We use the GRU in Google colab to perform computations. We compare our model with a simple linear regression. We were inspired by an article by Rohan Kosandal on Medium: 

https://medium.com/analytics-vidhya/weather-forecasting-with-recurrent-neural-networks-1eaa057d70c3

As we are restricted in our model complexity, we only use temperatures as our features. These are by far the most corelated features to the target variable. 

### Files 

The model is trained in the nn folder. We have the following files. 

- weather_nn_model.ipynb : Where the model is trained 
- lstm_model_test.ipynb: We test the LSTM model
- nn_linreg_compare.ipynb: We train a linear regression as a baseline model

### Architecture of the model 

Given the amount of data and complexity of LSTM networks we were limited in our choices of architecture, since we can only use Google colab for 12 hours. We play we different scenarios (not in the files) and decide to use the following architecture, which takes about 5 hours to train. 

```
Model: sequential_7
Layer (type)                 Output Shape              Param #   
=================================================================
bidirectional_7              (None, 96, 192)           75264     
dropout_22 (Dropout)         (None, 96, 192)           0         
lstm_23 (LSTM)               (None, 96, 96)            110976    
dropout_23 (Dropout)         (None, 96, 96)            0         
lstm_24 (LSTM)               (None, 96)                74112     
dropout_24 (Dropout)         (None, 96)                0         
dense_7 (Dense)              (None, 36)                3492      
=================================================================
Total params: 263,844
Trainable params: 263,844
Non-trainable params: 0
_________________________________________________________________
```



### Model Performance

We obtain the following scores. 

- Mean squared error on train set: 14.26 
- Mean squared error on test set: 13.33
- Mean absolute error on train set: 2.78
- Mean absolute error on test set: 2.71
- Mediane absolute error on train set: 2.24
- Mediane absolute error on test set: 2.21

A simple linear regression, using the same features, but taking data on the 10 previous days obtain: 

- Mean squared error on train set: 15.42
- Mean squared error on test set: 14.01
- Mean absolute error on train set: 2.88
- Mean absolute error on test set: 2.74
- Mediane absolute error on train set: 2.30
- Mediane absolute error on test set: 2.20

As a remark, one should be careful in comparing these numbers to those in the first project. In the first project, we only predict daily temperature for one day using many features. Here, we predict hourly temperatures for the next 36 hours, using only temperature data. There is much more variation. 

### Conclusion

We see that the RNN performs better than the simple linear regression. The RNN could also be improved, since our architecture is fairly simple. However, the linear regression only took a few seconds to train, as opposed to 5 hours for the RNN. We are limited in the complexity of the RNN we can train. There does not seem to be any overfitting issues, probably helped by the addition of dropouts. In order to improve the model, we could check the following: 
- Try different number of epochs. In this model we tried 20 epochs, and our model seemed to have stopped improving after ony a few epochs. The time saved could be put elsewhere. 
- Try different batch size. Our batch size was fairly large to save time, we could try to make it smaller, to see if there is a difference. 
- Play with the number of hours we use for prediction. We chose 96 hours here, again because we were limited in the time we can use the GPU. 
- Try a different RNN architecture. 
- Add more feature to our model, wind_speed for example seem to have some corelation. 

## Future Projects 

In the future, we would like to use our data to obtain predictions about 

- Predict daily precipitations, humidity and wind speed. 

The end goal is to create our own weather app in Montreal. 
