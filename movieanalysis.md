## Python Analysis: Movie Profitability

For this project, I analyzed a dataset on 7,668 movies from the years 1980 to 2020 to determine which category of movies tends to have the highest return on investment.  Specifically, I looked at which combination of genre and country of production had the highest return on investment, a) for high-budget movies and b) for low-budget movies.  I analyzed high- and low-budget movies separately because I was curious to see whether the ROIs would vary much between these two groups and because some genres, such as action movies, might not be profitable without having a large budget.  Based on the analysis, for high-budget movies, UK Adventure movies can be expected to have the greatest ROI. For low-budget movies, US Horror movies can be expected to have the greatest ROI. The ROI of low-budget US Horror movies is typically much greater than the ROI of high-budget UK Adventure movies.  25% of the low-budget US Horror films had an ROI of between 3.5 and 17 (i.e. 350% and 1700%), and another 25% had an ROI between 17 and 37.5!   
[Dataset](https://www.kaggle.com/danielgrijalvas/movies) | [Jupyter Notebook](https://github.com/cmkuhn01/cmkuhn01.github.io/blob/main/PythonStats_MovieAnalysis.ipynb)


### 1. Data Preparation

The dataset contains the movie title, genre, country of production, budget, total revenue, director, writer, star, production company, year of release, release date, runtime, IMDb score, number of IMDb user votes, and MPA rating.

The data was checked for duplicate entries: there were only 7,512 unique movie titles in the data, but the movies with duplicate titles were released in different years, so there were no true duplicate rows in the data.  The range of years was examined, as a "low budget" today would be a very high budget in the early days of movie-making.  The earliest movies in the dataset were from 1980.  For this analysis, no adjustments were made to account for inflation.  

For the high-budget subset, I used the 750 movies with the highest budgets.  For the low-budget dataset, I used the 750 movies with the lowest budgets of at least $1 million.  Within each subset, if the number of samples per country or per genre was very small, I removed that data from the subset, as such a small sample size would not yield a reliable conclusion.

For the high-budget films, the only countries with significant sample sizes were the U.S. and the U.K., and the genres with significant sample sizes were Action, Animation, Comedy, Adventure, and Drama. For the low-budget films, the countries with significant sample sizes were again the U.S. and the U.K.  The low-budget film genres with significant sample sizes were Comedy, Drama, Horror, Action, and Crime.  


### 2. Analysis
  
Scatterplots were made of total revenue vs. budgets of the high-budget movies, with the data points color-coded by country in one plot and by genre in the other.  There were no obvious trends by country or genre.  In general, total revenue increased with budget.  
  
The return on investment (ROI) was calculated as the difference between the gross revenue and the budget, divided by the budget.  For each combination of genre and country within the high-budget subset, the mean, standard deviation, maximum, and minimum ROI were calculated. Of the high-budget movies, U.K. Adventure movies had the highest mean ROI, but they also had a high standard deviation.  The ROIs for each combination of country and genre were plotted in boxplots for a more thorough comparison.  Each quartile bound of the U.K. Adventure movies was higher than that of the other genre-country categories.  The number of samples in UK Adventure is small (only 19), but they seem to be consistently successful relative to the other groups. The median ROI for UK Adventure movies is 3.2, with the middle 50% of ROIs being between 1.5 and 4.8.  The maximum ROI for UK Adventure movies is 9.8.  Based on this analysis, for high-budget movies, U.K. Adventure movies tend to have the highest ROI.   
  
<a href="images/movies_highbudgetROIs.png"><img src="images/movies_highbudgetROIs.png?raw=true"/></a>  
  
The same analysis approach was used for the low-budget movie dataset.  Again, no obvious trends appeared in the plots of total revenue vs. budget with the points color-coded by country or genre.  The ROI was calculated for each movie, and the mean, standard deviation, maximum, and minimum ROI were calculated for each combination of genre and country within the low-budget movies subset.  U.S. Horror movies had the highest mean ROI at 12.3 and the highest maximum ROI at 100.8.  U.K. Comedy movies also had high mean and maximum ROIs: 10.7 and 72.7, respectively.  The standard deviation in ROI for U.S. Horror movies was similar to that of U.K. Comedy movies.  The data were plotted in boxplots for a closer look.  In the boxplots, the trends become much clearer: U.S. Horror movies tend to have a much higher ROI than U.K. Comedies.  Of all of the genre-country combinations within the low-budget dataset, U.S. Horror was the only combination were less than 25% of movies had negative ROIs.   

<a href="images/movies_lowbudgetROIs.png"><img src="images/movies_lowbudgetROIs.png?raw=true"/></a>  

Additionally, the U.S. Horror films had the highest median and highest maximum relative to each of the other low-budget categories.  75% of the low-budget U.S. Horror movies had an ROI of at least 1 (i.e. profit was at least as much as investment), 25% had an ROI between 3.5 and 17 (i.e. 350% and 1700%), and another 25% had an ROI between 17 and 37.5.  
  
**Correlation with year was examined:**    
  
Almost all low-budget US Horror films had ROIs below 20 until 2010. ROI distribution for low-budget US Horror films is overall higher for the 2010-2020 range than for 1980-2010. Since 2010, only 3 data points in the low-budget US Horror category had negative ROIs. For low-budget Comedies and Dramas, the range of ROIs is also greater for 2010-2020 than for 1980-2010. However, there remain many data points with negative ROIs for Comedy and Drama in 2010-2020.  

<a href="images/ROIsvsyear.png"><img src="images/ROIsvsyear.png?raw=true"/></a>  
  
<a href="images/ROIsvsyeargenre.png"><img src="images/ROIsvsyeargenre.png?raw=true"/></a>  
  

**Correlation with other factors was considered:**  
  
Within the low-budget US Horror data, there are 78 titles with 70 different directors, 69 writers, 70 leading stars, and 60 different production companies. The "star" column only lists one actor/actress. For future work, the "star" column could be expanded to include the 5 top-billed stars for each movie, to more thoroughly check for correlations between ROI and star.
  

### 3. Conclusions

For high-budget movies, UK Adventure movies can be expected to have the greatest ROI. For low-budget movies, US Horror movies can be expected to have the greatest ROI. The ROI of low-budget US Horror movies is typically much greater than the ROI of high-budget UK Adventure movies.   
 


### 4. Future Work

1. Expand the dataset to include more movies: This dataset contains information on only 7,668 of the more than 8 million movies listed on IMDb. From the description on how the data was collected, as well as the analysis done here, it is not clear that there are any biases in which movies made it into the subset. However, the sample is very small relative to the total number of movies for which information is available.  A larger sample size may show different trends.  
  
2. Expand "star" data: get the 5 top-billed actors/actresses for each movie and re-examine for correlations to ROIs.  
  
3. Analyze franchises separate from series, separate from movies without sequels or first-in-a-series movies.
