# Weatherpredict

Weatherpredict is a machine learning project whose goal is to predict various weather datas. 

The project was inspired by a series of three articles by Adam McQuistan.

<https://stackabuse.com/using-machine-learning-to-predict-the-weather-part-1/>

The data set is different.

### Data set 

We obtained historical weather datas for Montreal from OpenWeatherMap. 
The datas are hourly information from January 1st, 1979 to July 31st, 2020. 

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

* cleaning_datas.ipynb : perform initial cleaning of the datas that will be used in our projects. 

## Project 1: Linear Regression model to predict temperatures

The goal is to use linear regression to predict the temperature on a given day.

We resample the hourly datas into hourly datas. 

We fit a linear regression model, a Ridge regression model and a Lasso regression model. The last two models were used more as educational purposes, as it was not expected that they would improve linear regression, since we had removed collinearity problems in the data cleaning and there was a clear linear trend between our target and feature variables.  

### Files 

We have the following files. 

* cleaning_datas_lin_reg.ipynb : perform data cleaning to prepare for a linear regression model;
* weather_lin_reg_model.ipynb : create linear regression models and analyse the datas. 
	
### Conclusion 

Our models did better than the 'naive model', where we simply predict that the temperature on a given day is the same as the previous day. 

We obtained the following: 

 - Explained Variance: 0.91
 - Mean Absolute Error: 2.55 degrees celsius
 - Median Absolute Error: 1.93 degrees celsius

There was no significant difference between Ridge regression and linear regression, probably 
in part due to the fact that we removed independant parameters with high correlation. 