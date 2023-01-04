# GDAC-Capstone
## Case Study 2: How can a wellness Technology Company play it smart?

### Bellabeat
This is an optional capstone project for the [Google Data Analytics certification](https://www.coursera.org/professional-certificates/google-data-analytics) through Coursera.

## Scenario: 
I am a junior data analyst working on the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. I have been asked to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights I discover will then help guide marketing strategy for the company. I will present your analysis to the Bellabeat executive team along with my high-level recommendations for Bellabeat’s marketing strategy.

### Bellabeat Products:
- Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.
- Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.
- Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.
- Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.
- Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.


This case study follows the 6 steps of the data analysis process:
### [ASK](#1-ask)
### [PREPARE](#2-prepare)
### [PROCESS](#3-process)
### [ANALYZE](#4-analyze)
### [SHARE](#5-share)
### [ACT](#6-act)



## 1. ASK
  1.1 - Sršen asks me to analyze smart device usage data in order to gain insight into how consumers use non-Bellabeat smart
devices. She then wants me to select one Bellabeat product to apply these insights to in your presentation. These questions
will guide my analysis:

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

The problem we are trying to solve is to discover new business opportunities for Bellabeat and their products based on how users use other smart fitness devices. The insights gained from this data would inform Bellabeat's marketing strategy and product plans. The key stakeholders are Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, and Sando Mur, the other co-founder. Secondary stakeholders are the Bellabeat marketing analytics team.

### Business Task:
I am asked to analyze publically available FitBit data to gain insight into how customers are using their smart devices, and then to present my findings and recommendations to Bellabeat's marketing team.

## 2. PREPARE
  ### 2.1 - Data set
  This data is publically available on [Kaggle](https://www.kaggle.com/datasets/arashnic/fitbit). This data set contains 18 tables in CSV format, housing data from       30 users. The 30 eligible FitBit users consented to submission of personal tracker data.
  
  Upon reviewing the tables, I found that the dailyActivitiesMerged table consists of combined data from dailyCaloriesMerged, dailyIntensitiesMerged, and dailyStepsMerged. With this in mind we can use the dailyActivitiesMerged table for our analysis and disregard these other tables.
  
  In reviewing other tables such as heartrate_seconds_merged and weightLogInfoMerged, there were only 14 and 8 different IDs, and therefor not a large enough sample size to continue analysis. After this review I landed on 4 main tables for my analysis:
  -  dailyActivity_merged
  -  sleepDay_merged 
  -  hourlyIntensity_merged
  -  hourlySteps_merged
    
  ### 2.2 - Does the data ROCCC?
  - Reliable - This data alone is not reliable, as the sample size of 30 users is not large enough to infer conclusions regarding the millions of smart fitness device users worldwide.
  - Original - This data is not original. It was supplied from a survey via Amazon Mechanical Turk, and not from FitBit themselves. 
  - Comprehensive - This data is not comprehensive. It would be better if it contained more data from more users to perform a more robust analysis.
  - Current - This data is not current. It was gathered over a 2 month period in the Spring of 2016.
  - Cited - This data is cited - it came from Amazon Mechanical Turk via survey. There are no privacy concerns as it is open source and the users consented to sharing their data.

  ### 2.3 Problems with the data
  - The Central Limit Theorem's general role of n >= 30 is met here on the tables I chose, however a larger sample size would be desirable. 
  - While reviewing the data, there are only 8 users who inputted data for weight. Of those 8 users, 5 inputted weight manually while 3 provided this data through some other device.
  - Overall this data presents problems in it being a final source of truth, but it could still provide insights that would beneficial to investigate further or even take under consideration.

## 3. PROCESS
### 3.1 Tools used
- SQL for data cleaning and analysis
- Tableau for visualization

### 3.2 Cleaning data
- Check and review nulls, remove duplicates, and ensure data is ready to analyze. This is done on the 4 tables selected.
```
--repeated for each column, discovered there are no nulls in the 4 tables.
select COLNAME
from TABLE
where COLNAME IS NULL

--discovering duplicates in tables
select id
,	count(distinct(ActivityDate)) AS dupeCheck
from dailyActivity_merged
group by ActivityDate, id
order by dupeCheck desc

--converting datatypes on a table
select Id
, cast(activitydate as date) AS newDate
, cast(totalSteps AS int) AS newTotalSteps
, cast(calories as smallint) AS newCalories
, round(totaldistance, 3) AS newTotalDistance
, round(trackerdistance, 3) AS newTrackerDistance
, round(LoggedActivitiesDistance, 3) AS newLoggedActivitiesDistance
, round(VeryActiveDistance, 3) AS newVeryActiveDistance
, round(ModeratelyActiveDistance, 3) AS newModeratelyActiveDistance
, round(LightActiveDistance, 3) AS newLightActiveDistance
, round(SedentaryActiveDistance, 3) AS newSedentaryActiveDistance
from [dbo].[dailyActivity_merged]
```
From here I felt confident that the data was ready to analyze.

## 4. ANALYZE

I began by finding the average steps, overall, from the data
```
--finding the overall average steps per id
select id
, avg(cast(totalsteps as int)) as avgSteps
from dailyActivity_merged
group by id
```
Which resulted in 

I then wanted to see what percentage of time in bed was categorized by sleep
```
select id
, cast(SleepDay as date) as newDate
, totalminutesasleep
, totaltimeinbed
, cast(TotalMinutesAsleep as float) / TotalTimeInBed as percentSleep
from sleepDay_merged
```

## 5. SHARE
## 6. ACT
