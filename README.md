# Power Outages
Power outages are typically known to be short and uneventful, as people are typically unaffected in the duration that they are unaffected. However, longer-lasting power outages can cause significantly more damage, especially since a lot of crucial processes need power in order to operate properly and effectively. Having the ability to determine how long a duration will last could significantly help people prepare for such an incident, as they would be able to decide what resources they would need in order to conduct themselves safely during an outage. When considering longer-lasting power outages, we ask whether more people are affected by longer power outages compared to short ones. Is there a relationship between the length of a power outage and how many people are affected?

We decided to research the 

 ---

## Introduction
The dataset we have implemented contains data ranging from 2000 to 2016. 

## Data Cleaning and Exploratory Data Analysis
To work with the data in an efficient manner, we had to first clean it up through several different steps so we could perform analysis on it to determine if there was a relationship with outage duration and the amount of people affected. 

### Initial Cleaning
We first started by importing the 'outage.xlsx' file. From there, we made sure that our DataFrame only contained information relevant to our research by eliminating columns such as the blank 'variables' column and skipping over the beginning metadata rows that did not contain informtion necessary.
   
### Timestamp Conversion
In order to precisely calculate outage durations, we converted the data by merging the date and time columns into single date-time columns - 'OUTAGE.START' and 'OUTAGE.END'. 

### Duration Calculation
From our timestamp columns, 'OUTAGE.START' and 'OUTAGE.END', we calculated the duration of each outage in the datetime format. Calculating the duration of all events was crucial in order to further perform exploratory data analysis and determine whether the outage duration affected other aspects of data, including how many people were affected.

Through these steps, we converted our initial dataset to one suitable for investigative data analysis.

  
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
This first histogram compares the amount of people affected by an outage duration with the duration of the incident. The distribution is relatively spread evenly, but is quite difficult to read, which is why we binned the data in the next graph to make more meaningful conclusions about the data at hand.
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
#### Model Parameters
#### Features and Preprocessing
#### Base Model Assessment
**Model Performance**

 ## Final Model

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
