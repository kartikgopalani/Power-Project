# Power Outages
Power outages are typically known to be short and uneventful, as people are typically unaffected in the duration that they are unaffected. However, longer-lasting power outages can cause significantly more damage, especially since a lot of crucial processes need power in order to operate properly and effectively. When considering longer-lasting power outages, we ask whether more people are affected by longer power outages compared to short ones. Is there a relationship between the length of a power outage and how many people are affected?

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
   
   
 ## Assessment of Missingness
 ## Hypothesis Testing
 ## Framing a Prediction Model
 ## Baseline Model
 ## Final Model
 ## Fairness Analysis
 
