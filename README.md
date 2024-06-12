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
From this chart, we can see the distribution of people affected by power outages. There are very few incidents that affected more than 0.5 million people - however, in certain outages, over 3 million were affected. 

### Bivariate Analysis
This histogram compares the amount of people affected by an outage duration with the duration of the incident. The data are binned to provide easier readability and to better determine if there is a correlation with the two variables. 
   
## Assessment of Missingness
### Is the 'CAUSE.CATEGORY.DETAIL' Column NMAR?
After initial analysis, we came to the conclusion that the 'CAUSE.CATEGORY.DETAIL' column of the dataset is likely NMAR, since the values missing in the column depend on the categories that they would be a part of. Outages that are started because of cyber attacks, or other similar issues that have to do with national security might be missing due to the sensitive nature of their causation. In the case that those in charge of releasing this information to the public do not want the cause of the outage known to the public for security purposes, the values would be missing based on the category they are a part of. Thus, we came to the conclusion that the missingness of the 'CAUSE.CATEGORY.DETAIL' column is NMAR. 
 
### Missingness Analysis of the 'OUTAGE.DURATION' Problem
We chose to analyze the missingness of the 'OUTAGE.DURATION' column to determine whether or not the duration of a power outage was missing at random based off other columns in the data. Additionally, assessing the missingness out the 'OUTAGE.DURATION' column would give us more insight on the missingness of other similar columns that had to do with time, such as the 'OUTAGE.START' column. 

In order to analyze missingness, we computed permutation tests comparing the 'OUTAGE.DURATION' column with two other columns: 'CLIMATE.CATEGORY' and 'CLIMATE.REGION'. 

**'CLIMATE.CATEGORY': P-Value= 0.002**
The P-Value of 0.002 for the 'CLIMATE.CATEGORY' permutation test suggests a high level of correlation between climate category and missingness of the related 'OUTAGE.DURATION' value. This implies that the 'OUTAGE.DURATION' column is MAR based on 'CLIMATE.CATEGORY'.

**'CLIMATE.REGION': P-Value= 0.165**
The P-Value of 0.165 for the 'CLIMATE.REGION' permutation test suggests that there is not a correlation between the climate region and missingness of the related 'OUTAGE.DURATION' value. Although there might be some correlation between the values, the results are not statistically significant enough to suggest that the 'OUTAGE.DURATION' column is MAR based on the 'CLIMATE.REGION' column.
 
 ## Hypothesis Testing
 #### Null Hypothesis (H0):
 The distribution of outage durations in the 'MRO' region is the same as the distribution of outage durations in all regions.

 #### Alternative Hypothesis (H1):
 The distribution of outage durations in the 'MRO' region is different from the distribution of outage durations in all regions.

 _threshold:_ 0.05%
 
 ## Framing a Prediction Model
 ## Baseline Model
 ## Final Model
 ## Fairness Analysis
 
