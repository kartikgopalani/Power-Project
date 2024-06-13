# Power Outages
Power outages are typically short inconveniences that lead people to lose access to various resources and infrastructure that they might require. In most cases, these outages are caused by severe weather patterns or over-use of the power grid. Having the ability to determine what caused a power outage could significantly help those in power to figure out how to minimize the duration of an outage. 

---

## Introduction
The dataset we have implemented contains data ranging from 2000 to 2016. Most power outages are typically caused by severe weather patterns, as storms such as tornadoes and hurricanes can significantly disrupt power sources necessary to light up millions of homes. However, in some cases power outages can be caused by other factors, such as intentional attacks. Having the ability to predict what causes an outage could be beneficial for city officials to inform residents how to conduct themselves safely.

We decided to study power outages based on whether they are caused by severe weather in this project, and we considered a variety of factors in our 1534-row dataset to better analyze the patterns displayed. In our dataset, exactly 0.5 of the outages were caused by severe weather. We utilized columns such as 'OUTAGE.START' and 'OUTAGE.RESTORATION' to determine the length of an outage, 'CLIMATE.REGION' and 'U.S._STATE' to determine where an outage happened, and 'CAUSE.CATEGORY' to see why an outage happened. 

## Data Cleaning and Exploratory Data Analysis
To work with the data in an efficient manner, we had to first clean it up through several different steps so we could perform analysis on it to determine if there was a relationship with outage duration and the amount of people affected. 

### Initial Cleaning
We first started by importing the 'outage.xlsx' file. From there, we made sure that our DataFrame only contained information relevant to our research by eliminating columns such as the blank 'variables' column and skipping over the beginning metadata rows that did not contain informtion necessary.
   
### Creating Columns
In order to precisely calculate outage durations, we converted the data by merging the date and time columns into single date-time columns - 'OUTAGE.START' and 'OUTAGE.RESTORATION'. From our timestamp columns, 'OUTAGE.START' and 'OUTAGE.RESTORATION', we calculated the duration of each outage in the datetime format, and saved it a column named 'Outage Duration DateTime'. We also made a column called 'Outage Duration', which is the primary column we use for our data analysis, and is of the type timedelta64. Calculating the duration of all events was crucial in order to further perform exploratory data analysis and determine whether the outage duration affected other aspects of data, including how many people were affected. Below is the head of our cleaned dataframe:

<iframe
  src="assets/dfhead.png"
  width="600"
  height="300"
  frameborder="0"
></iframe>

### Univariate Analysis
From this chart, we can see the distribution of power outage duration, and notice that almost all outages occured for less than 500 hours, with most occuring less than 200 hours. We can also see that there are certain outliers that might disrupt the accuracy of future data analysis that we conduct. 

<iframe
  src="assets/univariate_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From this next chart, we see the distribution of number of people affected by each outage. We can determine that very few incidents affected more than 0.5 million people, although a few affected even more than 3 million people.

<iframe
  src="assets/univariate_2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
This first graph compares the amount of people affected by an outage duration with the duration of the incident. The distribution is relatively spread evenly, but is quite difficult to read, which is why we binned the data in the next graph to make more meaningful conclusions about the data at hand.
<iframe
  src="assets/bivariate_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This histogram bins the data and sums the amount of people affected from an outage based on each bin. From this histogram, we can notice that the sum of people affected by an outage drops significantly after the duration length surpasses 250 hours. 
<iframe
  src="assets/bivariate_2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness


### Is the 'CAUSE.CATEGORY.DETAIL' Column NMAR?
After initial analysis, we came to the conclusion that the 'CAUSE.CATEGORY.DETAIL' column of the dataset is likely NMAR, since the values missing in the column depend on the categories that they would be a part of. Outages that are started because of cyber attacks, or other similar issues that have to do with national security might be missing due to the sensitive nature of their causation. In the case that those in charge of releasing this information to the public do not want the cause of the outage known to the public for security purposes, the values would be missing based on the category they are a part of. Thus, we came to the conclusion that the missingness of the 'CAUSE.CATEGORY.DETAIL' column is NMAR. 
 

### Missingness Analysis of the 'OUTAGE.DURATION' Problem
We chose to analyze the missingness of the 'OUTAGE.DURATION' column to determine whether or not the duration of a power outage was missing at random based off other columns in the data. Additionally, assessing the missingness out the 'OUTAGE.DURATION' column would give us more insight on the missingness of other similar columns that had to do with time, such as the 'OUTAGE.START' column. 

In order to analyze missingness, we computed permutation tests comparing the 'OUTAGE.DURATION' column with two other columns: 'CLIMATE.CATEGORY' and 'CLIMATE.REGION'. 

**'CLIMATE.CATEGORY': P-Value= 0.002**

The P-Value of 0.002 for the 'CLIMATE.CATEGORY' permutation test suggests a high level of correlation between climate category and missingness of the related 'OUTAGE.DURATION' value. This implies that the 'OUTAGE.DURATION' column is MAR based on 'CLIMATE.CATEGORY'.

A histogram displaying the empirical distribution of the climate category and our observed statistic is displayed below. From this graph, we can see that the observed statistic is significantly higher than the empirical distribution, showing how there must be a relationship between 'OUTAGE.DURATION' and 'CLIMATE.CATEGORY'.
<iframe
  src="assets/climate_cat2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


**'CLIMATE.REGION': P-Value= 0.165**

The P-Value of 0.165 for the 'CLIMATE.REGION' permutation test suggests that there is not a correlation between the climate region and missingness of the related 'OUTAGE.DURATION' value. Although there might be some correlation between the values, the results are not statistically significant enough to suggest that the 'OUTAGE.DURATION' column is MAR based on the 'CLIMATE.REGION' column.

## Hypothesis Testing
#### Null Hypothesis (H0):
The distribution of outage durations in the 'MRO' region is the same as the distribution of outage durations in all regions.

#### Alternative Hypothesis (H1):
The distribution of outage durations in the 'MRO' region is different from the distribution of outage durations in all regions.

_threshold:_ 0.05%
#### Methodology:
We chose to use the absolute difference in means as our test statistic in order to calculate if there was a significant difference in the distribution of outage durations for the 'MRO' region versus outage durations in all regions. We chose to use the absolute difference because we were not trying to see if the 'MRO' outage durations were larger or smaller than the general regions; we simply wanted to determine if distributions were different.

We based our distributions by randomly permuting the 'Outage Duration' column we computed earlier, and then took samples comparing durations where the region was 'MRO' with durations where the region was generalized. 

#### Conclusion: 
With a p-value of 0.2152, we fail to reject the null hypothess, as it is significantly higher than our threshold p-value of 0.05. We do not have sufficient data to prove that the distribution of outage durations in the 'MRO' region are significantly different from the durations of outages in all regions. Below, a Histogram is displayed comparing the observed statistic with the permuted statistics from our samples. 
<iframe
  src="assets/final_hyp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Model
From our earlier data analysis and reasoning, we came up with a new question to solve: _**Based on data available when a power outage initially begins, can we predict whether an outage was due to severe weather?**_
We used a Binary Classification Task, and utilized four features from the data: 'YEAR', 'MONTH', 'OUTAGE.START.TIME', and 'U.S._STATE'. These features enables to gain a better understanding of the time and region in which the power outages were occuring, and helped guide to create a model in which we could accurately predict whether an outage was caused by severe weather patterns. We will predict the response variable 'is_sw', which we created based off of the 'CAUSE.CATEGORY' column from the inital dataset. The response variable 'is-sw' returns 'True' if the outage is indeed caused by severe weather and 'False' if the outage is caused by another factor. Since we were primarily dealing with predicting the 'is_sw' response variable, we used binary classification instead of multi-class classification, as we believed we would gain more accurate results solely predicting a binary variable. 

The metric we are using to assess our model is accuracy. In our prediction model, we care most about having the ability to generalize correct predictions for both when the power outage is caused by severe weather and when it is not. Having a false positive result from our prediction would be problematic as it would lead officials to believe that an outage was weather-related, when it might be due to an intentional attack. This could slow down city officials' ability to find perpetrators, letting them more easily escape without getting in trouble with law enforcement. On the other hand, having a false negative result might lead officials to negate weather concerns, allowing residents of a given area to think it is safe for them to go outside when it might not be. This is especially a concern in remote areas, where inhabitants might not have access to news and resources that might tell them otherwise. Because both precision and recall are important to our prediction model, we decided to assess the model based on accuracy, which considers false positives and negatives with the same weight. 

## Baseline Model
#### Model Description
We chose to use the LogisticRegression classifier because our question was a binary classification question. Our model aims to predict whether a power outage was caused by a severe weather incident. We used the ‘saga’ solver, as that was the solver that gave us the best results, and chose a max_iter of 100 as we felt that was a good starting point for iterations.
#### Model Features and Preprocessing
We used two nominal (categorical) features: CLIMATE.CATEGORY and U.S._STATE. We picked those because we felt that geographical data would help our predictions: this data is also available at all times, as it is constant. We encoded both using a OneHotEncoder, dropping a column to avoid multicollinearity. 
#### Base Model Assessment
Out of all the recorded incidents, we found that half of them are caused by severe weather, which gave us a minimum required testing score of 0.5. Our model was able to consistently outscore that, with a test score of around **0.68** and a train score of around **0.70**. So, we consider a baseline model a ‘good’ model.


## Final Model
#### Model Features and Preprocessing
We chose to add three quantitative features, month, year and time. We had to create a helper function to help us encode time, transforming it into a 24-hour integer format. We chose these features because climate events can be tracked by month, for example, we suspect that more severe weather events happen in winter. We also chose year as a feature, because we suspect that climate change would have an impact on severe weather events. Finally, we chose time as a feature because it rains more frequently during the day, which could cause a severe weather event.
#### Final Model Assessment
We chose to use a similar LogisticRegression classifier, and we manually tested for our hyperparameter of max iteration. We found that a max iteration value of ~500 was ideal for our model.  Our final model had a test performance of around **0.73**, and a test performance of around **0.75**, a decent improvement from our baseline model. Therefore, we consider this model an improvement, and therefore a ‘good’ model. 

## Fairness Analysis

### Groups and Evaluation Metric

#### We created the groups using the 'UTIL.REALGSP' column
- **Group X:** States with a Real GSP contributed by the Utility industry (Measured in 2009 chained U.S. dollars) relative to the Real GSP contributed by the Utility industry of the general United States that is greater than 1. In other words, this group includes the states that have a higher median Real GSP from the Utility industry than that of the United States.
- **Group Y:** States with a Real GSP contributed by the Utility industry relative to the Real GSP contributed by the Utility industry of the general United States that is less than 1. In other words, this group includes the states that have a lower median Real GSP from the Utility industry than that of the United States.
- We used an evaluation metric of accuracy for this model. The accuracy will calculate the total amount of accurate predictions proportional to the total predictions made from the model. 

### Null and Alternative Hypotheses
- **Null Hypothesis (H0):** The model performance is fair in all groups. Our accuracy for predicting whether an outage is caused by severe weather is roughly similar for states with a lower Real GSP contributed by the Utility industry and higher Real GSP contributed to the Utility industry relative to the Real GSP contributed by the Utility Industry for the United States.
- **Alternative Hypothesis (H1):** The model performance is not fair across the groups. Our prediction model for assessing whether an outage was caused by severe weather is different for Group X and Group Y.

### Choice of Test Statistic and Significance Level
- **Test Statistic:** The absolute difference of the accuracy of Group X and Group Y.
- **Significance Level (α):** 0.05. This indicates that there is a 5% risk that we compute a difference in the models even if there is no difference.

### Resulting P-Value
- **p-value:** From our permutation test, we obtained a p-value, which shows the probability of observing a test statistic that is as great as, or greater than, the one computed if the null hypothesis was deemed true.

Since our P-Value is greater than 0.05, we fail to reject the null hypothesis. This means that we do not have sufficient evidence to conclude that the model is significantly different for states with a Real GSP from the Utility industry higher than the United States median and states with a Real GSP from the Utility industry lower than the United States median. Our fairness model's values are shown below:

| Metric               | Value                  |
|----------------------|------------------------|
| Accuracy (Higher)    | 0.7279                 |
| Accuracy (Lower)     | 0.7949                 |
| Observed Difference  | -0.0670                |
| p-value              | 0.2024                 |
