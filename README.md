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
- Check and remove duplicates. is is done on the 4 tables selected.
```
--finding duplicates in sleepDay_merged - yields 3 duplicates
select sd.*
FROM (
	SELECT id, sleepday
	FROM sleepDay_merged
	GROUP BY id, sleepday
	HAVING count(*) > 1) AS nsd
INNER JOIN  sleepDay_merged AS sd
ON sd.id = nsd.id
AND sd.sleepday = nsd.sleepday

--finding duplicates in dailyActivity_merged - none found
select da.*
FROM (
	SELECT id, activitydate
	FROM dailyActivity_merged
	GROUP BY id, ActivityDate
	HAVING count(*) > 1) AS nda
INNER JOIN  dailyActivity_merged AS da
ON da.id = nda.id
AND da.ActivityDate = nda.ActivityDate

--finding duplicates in hourlyIntensities_merged - none found
select hi.*
FROM (
	SELECT id, activityhour
	FROM hourlyIntensities_merged
	GROUP BY id, Activityhour
	HAVING count(*) > 1) AS nhi
INNER JOIN  hourlyIntensities_merged AS hi
ON hi.id = nhi.id
AND hi.Activityhour = nhi.Activityhour

--finding duplicates in hourlySteps_merged -- none found
select hs.*
FROM (
	SELECT id, activityhour
	FROM hourlySteps_merged
	GROUP BY id, Activityhour
	HAVING count(*) > 1) AS nhs
INNER JOIN  hourlySteps_merged AS hs
ON hs.id = nhs.id
AND hs.Activityhour = nhs.Activityhour
```
This shows that one table, sleepDay_merged, has 3 duplicates that will now be removed. I will do this with a CTE
```
---deleting the 3 duplicate entries. 3 rows affected.
WITH CTE (id
	, sleepday
	, totalsleeprecords
	, totalminutesasleep
	, totaltimeinbed
	, dupecount)
AS (SELECT id
		, sleepday
		, totalsleeprecords
		, totalminutesasleep
		, totaltimeinbed
		, ROW_NUMBER() OVER(PARTITION BY id
			, sleepday
			, totalsleeprecords
			, totalminutesasleep
			, totaltimeinbed
			ORDER BY id) AS dupecount
	FROM sleepDay_merged)
DELETE FROM CTE
WHERE dupecount >1
```
In reviewing the data I noticed data types are often incorrect (e.g, dates would be strings, not date). I want to cast these correctly before moving into analysis.
```
---correcting data types in dailyActivities_merged
ALTER TABLE dailyActivity_merged
ALTER COLUMN ActivityDate date;
ALTER TABLE dailyActivity_merged
ALTER COLUMN TotalSteps int;
ALTER TABLE dailyActivity_merged
ALTER COLUMN TotalDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN TrackerDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN LoggedActivitiesDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN VeryActiveDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN ModeratelyActiveDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN LightActiveDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN SedentaryActiveDistance float;
ALTER TABLE dailyActivity_merged
ALTER COLUMN VeryActiveMinutes int;
ALTER TABLE dailyActivity_merged
ALTER COLUMN FairlyActiveMinutes int;
ALTER TABLE dailyActivity_merged
ALTER COLUMN LightlyActiveMinutes int;
ALTER TABLE dailyActivity_merged
ALTER COLUMN SedentaryMinutes int;
ALTER TABLE dailyActivity_merged
ALTER COLUMN Calories int;
```
The other 3 tables had appropriate data types.

From here I felt confident that the data was ready to analyze.

## 4. ANALYZE

I began by performing a few aggregate functions on the tables
```
--finding the overall average steps
SELECT AVG(TotalSteps) AS AvgSteps
FROM dailyActivity_merged

--finding the overall average steps per ID
SELECT ID
, AVG(TotalSteps) AS AvgSteps
FROM dailyActivity_merged
GROUP BY ID

--finding average distance, per activity level by ID
SELECT ID
, AVG(VeryActiveDistance) AS AvgVeryActiveDist
, AVG(ModeratelyActiveDistance) AS AvgModActiveDist
, AVG(LightActiveDistance) AS AvgLightActiveDist
, AVG(SedentaryActiveDistance) AS AvgSedActiveDist
FROM dailyActivity_merged
GROUP BY ID

--finding average minutes per activity level, by ID
SELECT ID
, AVG(VeryActiveMinutes) AS AvgVeryActiveMinutes
, AVG(FairlyActiveMinutes) AS AvgModMinutes
, AVG(LightlyActiveMinutes) AS AvgLightMinutes
, AVG(SedentaryMinutes) AS AvgSedMinutes
FROM dailyActivity_merged
GROUP BY ID

--finding any relation between total steps in a day and total minutes asleep on that day
SELECT sd.ID
, ActivityDate
, TotalSteps
, TotalMinutesAsleep
FROM sleepDay_merged AS sd
INNER JOIN dailyActivity_merged AS da
ON sd.id = da.id
AND da.ActivityDate = sd.SleepDay

--finding the total steps per day/hour of the entire group
SELECT ActivityHour
,SUM(StepTotal) AS TotalSteps
FROM hourlySteps_merged
WHERE StepTotal >0
GROUP BY ActivityHour
ORDER BY totalsteps desc

--finding the average minutes asleep of the group
SELECT AVG(totalminutesasleep) AS totalSleepMinutes
FROM sleepDay_merged

--finding the average minutes per activity level overall
SELECT AVG(sedentaryminutes) AS AvgSedMin
, AVG(lightlyactiveminutes) AS AvgLightMin
, AVG(FairlyActiveMinutes) AS AvgFairMin
, AVG(VeryActiveMinutes) AS AvgVeryActiveMin
FROM dailyActivity_merged
```
I found a few insights from this analysis. The average user in this data set is taking 7,637 steps per day, which is below the recommended 10,000 steps to be considered active. On average, the users in this dataset slept 6.98 hours per night, which is near but lower than the recommended 7 hours per night (https://www.cdc.gov/sleep/about_sleep/how_much_sleep.html).

Additionally, I found 991 average sedentary minutes in the group, by far the largest activity level logged. I also created a visualization of when during the day most activity occurs. I also discovered that heaviest tracking was done Tuesday-Thursday, with drop offs of tracking info over the weekends during the survey.

## 5. SHARE
[Link to Tableau Dashboard](https://public.tableau.com/app/profile/matt.sowin/viz/GDACCapstone_16729407337290/GDACdashboard)

## 6. ACT
### 6.1 Final Conclusion
Based on my analysis of the public FitBit data provided, I found that on average, users underperform on recommendations regarding steps per day and hours of sleep per night. Bellabeat could find a way to incentivize a healthy lifestyle with this in mind. Additionally, there is a drop off of activity logged over weekends. I'm unsure if this is due to not wearing the device over the weekend, so finding a way to incentivize utilizing the tool all the time to get the most of it. 

With this I would recommend a few things to Bellabeat's marketing team:
- create push notifications to remind users to exercise in some way, especially during times determined to be lower than average. This should also be done on weekends as that's where the largest drop off occurs. 
- create benchmarks in the app around recommended steps per day based on weight loss goals, and remind users of how they are tracking against their goals. 
