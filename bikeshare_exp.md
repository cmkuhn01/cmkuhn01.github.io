## Data Exploration and Linear Regressions: Korean Bike Share

In this project, I plotted a 14-column dataset about Seoul bike share rentals to understand the distributions of the independent variables as well as the distribution of the number of bike rentals.  I also generated several linear regressions using different subsets of input variables and using the moving-average and moving-maximum of bike rentals.  While the linear regressions did not yield a good fit for the number of bikes rented per hour, the one-day average, 7-day average, 30-day average, one-day max, 7-day max, and 30-day max rentals were all very well-approximated using linear regression.   
[Dataset](https://archive.ics.uci.edu/ml/datasets/Seoul+Bike+Sharing+Demand) | [MATLAB Code](LinRegAndNN_KoreanBikeshare.pdf)


### 1. Data Preparation

The columns of the raw dataset were the date (in year-month-day format), count of bikes rented during each hour, hour of the day, temperature (C), humidity (%), windspeed (m/s), visibility (10m), dewpoint temperature (C), solar radiation (MJ/m2), rainfall (mm), snowfall (cm), season, holiday, and functional day.  The date was converted into an integer using MATLAB's datenum format; once in integer form, the date is able to be used as an input variable for regression or neural network models.  The season column, holiday column, and functioning day column were converted into doubles using MATLAB's categorical function.  Since the season column contained four categories, and the numbers representing each category are arbitrary, three new columns were created: winter, spring, and summer.  For each of these categories, the data was either 1 or 0 depending on whether the datapoint was in the given season.  A column for fall would have been redundant with the other three (a 0 in all three columns means that the season is fall).  
  

### 2. Data Exploration

A scatter plot of the original output data (the number of bikes rented per hour) yields a very spiky distribution, with much lower values in December and April than in the rest of the year.  I expanded the scale of the x-axis by subdividing the data into several separate plots.  The resulting plots show a seven-day pattern of five days each with a peak in the morning and another peak in the evening, followed by two days each with a single, smaller peak in the afternoon.  The five days with two peaks per day were weekdays, and the two days with one peak per day were weekends.  For each weekly subset, the magnitudes of the peaks were similar for all five workdays within the week.  
  
I calculated and plotted the rolling one-day average, 7-day average, 30-day hourly, one-day maximum, 7-day maximum, 30-day maximum hourly bike rentals. The average rentals could be enough to predict weekly or monthly revenue, assuming the rental fee is a constant.  The maximum rentals per hour could be more important than the average rentals if the focus is ensuring that enough capacity will be available.  The 1-day average rentals is much smoother than the hourly data, but still has sharp spikes.  The lower-resolution curves of the 7-day average or 30-day average hourly bike rentals are likely much easier to model than the original hourly counts.    
   
To get a sense of the distribution of data within each input variable, I created a scatter plot of each variable versus time.  To investigate possible explanations for the sharper spikes in the raw and 1-day averaged rental data, I plotted the 1-day average and 1-day maximum alongside the rainfall, snowfall, holiday, and functional day data.  The non-functional days and spring/summer rainfall days seem to be correlated with the spikier portions of the 1-day average and 1-day maximum bike rental distribution.  

<a href="images/bike3models_actualandpredicted.png"><img src="images/bike3models_actualandpredicted.png?raw=true"/></a>  

<a href="images/bike3models_actualvspredicted.png"><img src="images/bike3models_actualvspredicted.png?raw=true"/></a>  

<a href="images/bike3models_residuals.png"><img src="images/bike3models_residuals.png?raw=true"/></a>



### 3. Clarifying Questions

Answers to the following questions would add value to the existing data and allow for improved modeling:  
  
1.  What is the definition of a non-functional day?  
  
2.  Do the bike rental counts reflect how many bike rentals are initiated each hour, or do the counts also include rentals that were initiated in previous hours?  
  
3.  What is the structure of the rentals (for example, are they rented for certain brackets of time)?  Knowing whether the bikes can be rented for a full day or full week could impact the design of the model.  
  
4.  Are the same number of bikes available for rent each day of the year?  If not, the count of bikes offered for rent each day could be an important input factor to the model, in that it limits the maximum rentals.  
  
5.  Is data available on number of bikes returned during each hour?  The number of bikes available per hour could be an important input into the model.  
  


### 4. Analysis

**Linear Regressions with Solar Radiation as the Only Input**  
First, because the plot of the solar radiation levels per hour is similar in shape to the plot of the hourly bike rentals, I ran a linear regression of the hourly rentals on the hourly solar radiation level.  This resulted in a very poor fit, with an R-squared value of 0.07.  
A linear regression of the 1-day maximum rentals on the 1-day maximum solar radiation resulted in a better fit, with an R-squared value of 0.41, and the linear regression of the 7-day maximum rentals on the 7-day maximum solar radiation yielded an R-squared value of 0.66.

**Linear Regressions with Three Inputs**  
Next, the 7-day maximum temperature and 7-day mean humidity were incorporated, so that the linear regression of the 7-day maximum rentals was a function of the 7-day maximum solar radiation, 7-day maximum temperature, and 7-day mean humidity.  This fit resulted in an R-squared value of 0.79 and an RMSE of 448.  As shown in the plot below, the trends are generally predicted, though the actual distribution is smoother than the predicted distribution during most months, and the large dip in rentals between July and September is not represented in the predicted distribution.  

**Linear Regressions with All Inputs**  
The linear regression of the raw hourly bike rental counts as a function of all inputs yielded an R-squared value of 0.55.  Six additional regressions were run on the smoothed data, where both the inputs and the outputs used in each regression were smoothed by taking the 1-day average, 1-day maximum, 7-day average, 7-day maximum, 30-day average, or 30-day maximum, respectively.  Below is a table of the resulting R-squared values and RMSE values.  

|  Input & Output Smoothing | R-squared |     RMSE     |
| :-----------: | :-----------: | :----------:  |
| None (Raw Data)  | 0.55  |  431 |
| 1-Day Average  | 0.83  |  170  |
| 1-Day Maximum | 0.71 | 499 |
| 7-Day Average | 0.90 | 109  |
| 7-Day Maximum | 0.88 | 346 |
| 30-Day Average | 0.97  | 54  |
| 30-Day Maximum | 0.95 | 220 | 

A plot of the linear regression coefficients for each of these fits shows significant variation in the value of each coefficient across the 7 regressions and significant variation in the trends of which factors are dominant in each regression.  This could be because of collinearities in the input variables.  It could also be a result of the smoothing (averaging or taking the maximum) having a significantly different impact on the distribution of each variable.  

Below are the plots of the two regressions with the lowest RMSEs found in this analysis: the 7-day average hourly rentals as a function of all of the 7-day averaged input variables, and the 30-day average hourly rentals as a function of all of the 30-day averaged inputs.  These regressions would be valuable in estimating the weekly or monthly revenue.  

>> insert plot of 7 day avg reg and 30-day avg reg


### 5. Future Work  

1.  Test for collinearity in the input variables and generate regressions after collinearities are removed.  
2.  Examine distribution of each input variable after applying rolling average or rolling maximum function.  
3.  Add a column that tells whether the day is a weekend.  Possibly also add columns to specify the day of the week.   
