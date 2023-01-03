# GDAC-Capstone
## Case Study 2: How can a wellness Technology Company play it smart?

### Bellabeat
This is an optional capstone project for the [Google Data Analytics certification](https://www.coursera.org/professional-certificates/google-data-analytics) through Coursera.

### Bellabeat Products:
- Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.
- Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.
- Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.
- Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.
- Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.

### Tools
1. SQL for data cleaning and analysis
2. Tableau for data visualization

This case study follows the 6 steps of the data analysis process:
### [ASK](#1-ask)
### [PREPARE](#2-prepare)
### [PROCESS](#3-process)
### [ANALYZE](#4-analyze)
### [SHARE](#5-share)
### [ACT](#6-act)

## Scenario: 
I am a junior data analyst working on the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. I have been asked to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights I discover will then help guide marketing strategy for the company. I will present your analysis to the Bellabeat executive team along with my high-level recommendations for Bellabeat’s marketing strategy.

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
  - This data is publically available on [Kaggle](https://www.kaggle.com/datasets/arashnic/fitbit). This data set contains 18 tables in CSV format, housing data from       30 users. The 30 eligible FitBit users consented to submission of personal tracker data.
    
  ### 2.2 - Does the data ROCCC?
  - Reliable - This data alone is not reliable, as the sample size of 30 users is not large enough to infer conclusions regarding the millions of smart fitness device users worldwide.
  - Original - This data is not original. It was supplied from a survey via Amazon Mechanical Turk, and not from FitBit themselves. 
  - Comprehensive - This data is not comprehensive. It would be better if it contained more data from more users, including things like height and weight, to perform a more robust analysis.
  - Current - This data is not current. It was gathered over a 2 month period in the Spring of 2016.
  - Cited - This data is cited - it came from Amazon Mechanical Turk via survey.

  ### 2.3 Problems with the data
  - The Central Limit Theorem's general role of n >= 30 is met here, however a larger sample size would be desirable. As mentioned, data is available for only 30 users over a 2 month period. 
  - While reviewing the data, there are only 8 users who inputted data for weight. Of those 8 users, 5 inputted weight manually while 3 provided this data through some other device.
  - Overall this data presents problems in it being a final source of truth, but it could still provide insights that would beneficial to investigate further or even take under consideration.

## 3. PROCESS
## 4. ANALYZE
## 5. SHARE
## 6. ACT
