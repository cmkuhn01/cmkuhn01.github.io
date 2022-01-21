## Linear Regression and Neural Nets: Korean Bike Share

In this project, I analyzed a dataset on Seoul bike sharing demand to determine whether the included data could be used to predict how many bikes are rented during a particular hour of a particular day.  I fit the data using linear regression, a single-hidden-layer neural net, and various two-hidden-layer neural nets.  The best RMSE achieved was 283 bikes.  This RMSE is high compared to the typical usage December through March, but small compared to typical usage during the rest of the year.  Future work could explore the effects of varying the number of nodes in the neural net layers or of using a different type of activation layer in the neural nets.  Alternatively, an initial objective could be to predict the number of bikes rentals per day, week, or month.  Considering that the available data only covers a single year, achieving a very good fit on a very fine resolution might not actually improve the quality of future predictions.  If the data are available, year-to-year variation should be explored.  
[Dataset](https://archive.ics.uci.edu/ml/datasets/Seoul+Bike+Sharing+Demand) | [MATLAB Code]


### 1. Data Preparation

The columns of the raw dataset were the date (in year-month-day format), count of bikes rented during each hour, hour of the day, temperature (C), humidity (%), windspeed (m/s), visibility (10m), dewpoint temperature (C), solar radiation (MJ/m2), rainfall (mm), snowfall (cm), season, holiday, and functional day.  The date was converted into an integer using MATLAB's datenum format; once in integer form, the date is able to be used as an input variable for regression or neural network models.  The season column, holiday column, and functioning day column were converted into doubles using MATLAB's categorical function.  Since the season column contained four categories, and the numbers representing each category are arbitrary, three new columns were created: winter, spring, and summer.  For each of these categories, the data was either 1 or 0 depending on whether the datapoint was in the given season.  A column for fall would have been redundant with the other three (a 0 in all three columns would mean that it is fall). 
  
The output and each of the inputs were plotted as scatterplots, histograms, and boxplots to get a sense of the distributions of each variable.  There were outliers for windspeed, visibility, rainfall, snowfall, and bikes rented.  Rain and snow were rare in the dataset, so the fact that these datapoints were outliers does not mean that they would mislead the model.  Similarly, the visibility and solar radiation data were very heavily concentrated on a single value.  The atypical visibility and solar radiation levels were assumed to potentially be important in forming the model, so these data were kept in the dataset.  The outliers in number of bikes rented were when the number of rentals was especially high.  Predicting when the rentals will be especially high is important in being prepared have enough supply on hand, so the rental outliers were included in the model.  
  
Scatterplots of each pair of variables (variable A vs. variable B) were visually analyzed for correlations between variables.  Temperature and dewpoint appeared to be strongly linearly correlated.  Day of the year was strongly correlated with both temperature and dewpoint.  Hour of the day was strongly correlated with solar radiation level.  Weak relationships existed between humidity and dewpoint; temperature and solar radiation; and humidity and solar radiation.  The number of bike rentals was weakly correlated with day of the year, hour of day, temperature, dewpoint, and solar radiation.  
  
The correlation matrix was calculated and plotted as a heatmap, and the variance inflation factor was calculated for each input variable.  The day of year, temperature, humidity, dew point, and winter data each had a VIF > 10, indicating that there was strong multicollinearity.  Dewpoint had the highest VIF value, so dewpoint was removed from the dataset, and the correlation matrix and VIFs were calculated again.  With dewpoint removed, the day of year, winter, and spring data each had a VIF > 10.  Winter had the highest VIF in this case, so it was removed from the dataset.  With winter and dewpoint removed from the dataset, the correlation matrix and VIF for the remaining variables indicated no strong collinearity.  Therefore, all of the remaining variables were used in the regression and neural network analysis.  


### 2. Analysis

Three types of models were generated: a linear regression model, single-hidden-layer neural networks, and two-hidden-layer neural networks.  
  
The linear regression model used all 13 remaining input variables (excluding dewpoint and winter) and yielded an R-squared value of 0.54 and an RMSE of 437.  The plot of actual vs. predicted values shows that the prediction performance is very poor when the actual number of bike rentals is very high.  The maximum predicted value was less than 2000, while the maximum actual number of rentals was greater than 3500.    
  
For the neural network models, the randomized dataset was split into a training set (30% of the datapoints), validation set (55%), and test set (15%).  The original input variable data varied widely in order of magnitude, so the mapstd function was used to standardize the data.  
  
Three cases were run for the single-hidden-layer neural network: 10 nodes, 50 nodes, and 100 nodes in the hidden reLu activation layer.  The neural network structure used a reLu layer and the adam training algorithm with initial learning rate set to 0.01, learning rate drop period set to 5 epochs, and learning rate drop factor set to 0.5.  Each of the 3 single-layer models resulted in a similar RMSE of approximately 400.  
  
Deeper neural networks allow for more nonlinearities to be captured by the model.  Neural networks containing two hidden reLu activation layers were trained and compared in terms of RMSE.  The first hidden layer contained 100 nodes and the second hidden layer contained 50 nodes.  The lowest RMSE, 283, was achieved using a batch size of 64 and a learning rate drop factor of 0.75.  Batch sizes of 32, 128, and 256, and learning rate drop factors of 1, 0.9, and 0.5 were also used, in all combinations, because it is difficult to predict which values of hyperparameters will provide the best-trained model.  
  
The predicted values and actual values of bike rentals were compared for each model: linear regression, best single-hidden-layer neural net, and best two-hidden-layer neural net.  As shown in the plots below, the two-hidden-layer neural net performed the best in terms of predicting the shape of the distribution and the range of the distribution.  The linear regression model and single-hidden-layer model were both unable to predict the higher values of bike rentals; however, the single-hidden-layer was much better than the linear regression model in predicting the overall shape and spread of the distribution of hourly rentals.  This is also evidenced in the residuals histograms, where the peak is much narrower for the two-hidden-layer model than for the linear regression model.
  
<img src="images/bike3models_actualandpredicted.png?raw=true"/>  

<img src="images/bike3models_actualvspredicted.png?raw=true"/>  

<img src="images/bike3models_residuals.png?raw=true"/>



### 3. Conclusions

While the RMSEs of the three types of models were all similar: 437 for the linear regression, 408 for the single-hidden-layer neural net, and 283 for the two-hidden-layer neural net, the scatter plots and residual histograms demonstrate that the two-hidden-layer neural net yields much more accurate predictions of the trends in hourly rentals, as compared to the linear regression and single-hidden-layer neural net.  


### 4. Future Work

Future work could explore the effects of varying the number of nodes in the neural net layers or of using a different type of activation layer in the neural nets.  Alternatively, an initial objective could be to predict the number of bike rentals per day, week, or month rather than per hour.  Considering that the available data only covers a single year, achieving a very good fit on a very fine resolution might not actually improve the quality of future predictions.  If the data are available, year-to-year variation should be explored.  The reliability of the models produced here could be tested by trying to predict the other years' data, and the additional years of data could be used in the training and validation sets to develop a more robust prediction model.  
