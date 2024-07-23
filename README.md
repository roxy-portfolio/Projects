# How Can a Wellness Technology Company Play It Smart?
## Bellabeat Case Study
![images](https://github.com/user-attachments/assets/253b249a-e864-40a0-b6d9-56f8ddb5972f)

I will be working with the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused
products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the
global smart device market. 

Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart
device fitness data could help unlock new growth opportunities for the company. I have to focus on one of
Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The
insights you discover will then help guide marketing strategy for the company. 

## Guided questions for the analysis: 
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

## Dataset used
[FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) (CC0: Public Domain, dataset made available through Mobius)

## Data Exploration
BigQuery and Google Sheets will be utilized for data processing and analysis, while Tableau will be used for data visualization.

The file caused an import error when using the "Auto detect" schema due to data type discrepancies, particularly with the timestamp format, which included AM/PM designations—formats not accepted by BigQuery. To resolve these issues and ensure a successful import, a custom schema was required. The CSV file was initially opened as a text document to inspect column names. Columns were then added individually using the "Add field" function (indicated by a plus sign in the schema section), with special attention given to specifying the timestamp column as a string field. For the other files, data types were changed to Datetime or Date using Google Sheets.

#### Checking unique user IDs for each tables 
````sql
SELECT
 DISTINCT Id
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`
````
| Table Name | Unique IDs | 
| ------- | ----------- |
| dailyActivity | 33 |
| heartrate_seconds | 14 |
| hourlyCalories | 33
| hourlyIntensities | 33 |
| hourlySteps | 33 |
| sleepDay | 24 |
| weightLoginfo | 8 |

## Data Cleaning & Manipulation

- The dataset shows that all tables have a common 'Id' column and a similar date column. These columns will be used to join the tables, reducing the total number from five to two: one for daily data (dailyActivity) and another for hourly data (hourlyActivity). Additionally, columns for days of the week (e.g., Monday, Tuesday) and time periods (e.g., Morning, Afternoon) will be created.

#### Data cleaning steps: 
- Deleted duplicate rows in the sleepDay table.
- Created new columns: ActivityDay, ActivityDate, ActivityHour and Period 
- New daily_activity table with sleepDay data columns added 
- Created hourly_activity table with data from hourlyCalories, hourlyIntensities and hourlySteps
  
_`*` represent new columns_
  
| Column Name | Action Taken |
| ------- | ----------- |
| ActivityDay* | Calculated by extracting the weekday from columns 'ActivityDate' or 'ActivityHour' (e.g., “Monday”) |
| ActivityDate* | Calculated by extracting the date from 'ActivityHour' datetime column |
| ActivityHour* | Calculated by extracting the time from 'ActivityHour' datetime column |
| Period* | This column is determined based on the time of day in the 'ActivityHour' column |
| Morning | 5AM to 11AM |
| Afternoon | 12PM to 6PM |
| Evening | 7PM to 10PM |
| Night | 11PM to 4AM |


````sql
CREATE OR REPLACE TABLE `omega-terrain-424207-q5.bellabeat.daily_activity` AS 
SELECT 
  day.Id,
  day.ActivityDate,
  FORMAT_DATE('%A', day.ActivityDate) AS ActivityDay,
  day.SedentaryMinutes,
  day.LightlyActiveMinutes,
  day.FairlyActiveMinutes,
  day.VeryActiveMinutes,
  day.SedentaryActiveDistance,
  day.LightActiveDistance,
  day.ModeratelyActiveDistance,
  day.VeryActiveDistance,
  day.LoggedActivitiesDistance,
  day.TrackerDistance,
  day.TotalDistance,
  day.TotalSteps,
  day.Calories,
  sleep.TotalSleepRecords,
  sleep.TotalMinutesAsleep,
  sleep.TotalTimeInBed
FROM `omega-terrain-424207-q5.bellabeat.daily_activity` AS day
LEFT JOIN `omega-terrain-424207-q5.bellabeat.sleep_day` AS sleep
ON day.Id = sleep.Id AND day.ActivityDate = sleep.SleepDay
````

- Create new hourly_activity table from the other 3 tables and split ActivityHour column into Date and Time columns.
- Create a period columns with day periods, i.e., Morning, Afternoon, etc.


````sql
CREATE OR REPLACE TABLE `omega-terrain-424207-q5.bellabeat.hourly_activity` AS
SELECT 
  calories.Id,
  DATE(calories.ActivityHour) AS ActivityDate,
  TIME(calories.ActivityHour) AS ActivityHour,
  FORMAT_DATE('%A', calorie FROM calories.ActivityHour) BETWEEN 12 AND 18 THEN 'Afternoon'
    WHEN EXTRACT(HOUR FROM calories.ActivityHour) BETWEEN 19 AND 22 THEN 'Evening'
    ELSE 'Night'
  END AS Period,
  calories.Calories,
  intensities.TotalIntensity,
  intensities.AverageIntensity,
  steps.StepTotal
FROM `omega-terrain-424207-q5.bellabeat.hourly_calories` AS calories


FULL OUTER JOIN `omega-terrain-424207-q5.bellabeat.hourly_intensities` AS intensities
ON calories.Id = intensities.Id AND calories.ActivityHour = intensities.ActivityHour
FULL OUTER JOIN `omega-terrain-424207-q5.bellabeat.hourly_steps` AS steps
ON calories.Id = steps.Id AND calories.ActivityHour = steps.ActivityHour
````

## Analyzing Trends and Insights

#### To effectively analyze the data for the Bellabeat case study, the following steps will be taken:

- Aggregate the data: Collect and compile the data to ensure it is useful and easily accessible for analysis.
- Organize and format the data: Systematically organize and properly format the data to facilitate smooth and efficient analysis.
- Perform calculations: Conduct necessary calculations to derive meaningful metrics and insights from the data.
- Identify trends and relationships: Analyze the data to uncover significant trends and relationships that can inform our conclusions and recommendations.

#### The analysis will be guided by the following principles:

Source: [World Health Organization](https://www.who.int/publications/i/item/9789240015128)
![images](https://cdn.who.int/media/images/default-source/health-topics/physical-activity/summary-infographic-guideline-on-physical-activity.jpg?sfvrsn=246f54b7_9)

ADULTS (18 years and older)

It is recommended that:

- Adults should do at least 150 – 300 minutes of moderate-intensity aerobic physical activity; or at least 75–150 minutes of vigorous intensity aerobic physical activity; or an equivalent combination of moderate- and vigorous-intensity activity throughout the week, for substantial health benefits.
  
- Adults who took 8,000 or more steps a day had a reduced risk of death over the following decade than those who only walked 4,000 steps a day. Source: [NIH Research Matters](https://www.nih.gov/news-events/nih-research-matters/number-steps-day-more-important-step-intensity)
  
- Step intensity (number of steps per minute) didn’t influence the risk of death, suggesting that the total number of steps per day is more important than intensity. Source: [NIH Research Matters](https://www.nih.gov/news-events/nih-research-matters/number-steps-day-more-important-step-intensity)



````sql
SELECT 
  MIN(SedentaryMinutes) AS MIN_SedentaryMinutes,
  MIN(LightlyActiveMinutes) AS MIN_LightlyActiveMinutes,
  MIN(FairlyActiveMinutes) AS MIN_FairlyActiveMinutes,
  MIN(VeryActiveMinutes) AS MIN_VeryActiveMinutes,
  MIN(SedentaryActiveDistance) AS MIN_SedentaryActiveDistance,
  MIN(LightActiveDistance) AS MIN_LightActiveDistance,
  MIN(ModeratelyActiveDistance) AS MIN_ModeratelyActiveDistance,
  MIN(VeryActiveDistance) AS MIN_VeryActiveDistance,
  MIN(TotalDistance) AS MIN_TotalDistance,
  MIN(TotalSteps) AS MIN_TotalSteps,
  MIN(Calories) AS MIN_Calories,
  MIN(TotalSleepRecords) AS MIN_TotalSleepRecords,
  MIN(TotalMinutesAsleep) AS MIN_TotalMinutesAsleep,
  MIN(TotalTimeInBed) AS MIN_TotalTimeInBed
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`
````

For full results: [click here](https://drive.google.com/file/d/1OT2dSiS7nMdL_C22FusTY-kp6XGgUeoF/view)

````sql
SELECT 
  AVG(SedentaryMinutes) AS AVG_SedentaryMinutes,
  AVG(LightlyActiveMinutes) AS AVG_LightlyActiveMinutes,
  AVG(FairlyActiveMinutes) AS AVG_FairlyActiveMinutes,
  AVG(VeryActiveMinutes) AS AVG_VeryActiveMinutes,
  AVG(SedentaryActiveDistance) AS AVG_SedentaryActiveDistance,
  AVG(LightActiveDistance) AS AVG_LightActiveDistance,
  AVG(ModeratelyActiveDistance) AS AVG_ModeratelyActiveDistance,
  AVG(VeryActiveDistance) AS AVG_VeryActiveDistance,
  AVG(TotalDistance) AS AVG_TotalDistance,
  AVG(TotalSteps) AS AVG_TotalSteps,
  AVG(Calories) AS AVG_Calories,
  AVG(TotalSleepRecords) AS AVG_TotalSleepRecords,
  AVG(TotalMinutesAsleep) AS AVG_TotalMinutesAsleep,
  AVG(TotalTimeInBed) AS AVG_TotalTimeInBed
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`
````
For full results: [click here](https://drive.google.com/file/d/1K2EQa4JwNfrFmxBE3EHoypqsKSDG0OVM/view)

````sql
SELECT 
  MAX(SedentaryMinutes) AS MAX_SedentaryMinutes,
  MAX(LightlyActiveMinutes) AS MAX_LightlyActiveMinutes,
  MAX(FairlyActiveMinutes) AS MAX_FairlyActiveMinutes,
  MAX(VeryActiveMinutes) AS MAX_VeryActiveMinutes,
  MAX(SedentaryActiveDistance) AS MAX_SedentaryActiveDistance,
  MAX(LightActiveDistance) AS MAX_LightActiveDistance,
  MAX(ModeratelyActiveDistance) AS MAX_ModeratelyActiveDistance,
  MAX(VeryActiveDistance) AS MAX_VeryActiveDistance,
  MAX(TrackerDistance) AS MAX_TrackerDistance,
  MAX(TotalDistance) AS MAX_TotalDistance,
  MAX(TotalSteps) AS MAX_TotalSteps,
  MAX(Calories) AS MAX_Calories,
  MAX(TotalSleepRecords) AS MAX_TotalSleepRecords,
  MAX(TotalMinutesAsleep) AS MAX_TotalMinutesAsleep,
  MAX(TotalTimeInBed) AS MAX_TotalTimeInBed
FROM `omega-terrain-424207-q5.bellabeat.daily_activity
````

For full results: [click here](https://drive.google.com/file/d/1d5Qj3ak-UpVUk0M0oPHUJTjPOBmlTFJb/view)

- Based on our findings, the average user takes approximately 7,652 total steps per day, sleeps for about 420 minutes or 7 hours, burns around 2,307 calories, and has between 14 to 21 minutes of fairly to very active minutes each day.
- These metrics provide a comprehensive view of user activity and lifestyle, highlighting the balance between physical activity, sleep, and calorie expenditure.


````sql
SELECT
  Id,
  ROUND(AVG(FairlyActiveMinutes) + AVG(VeryActiveMinutes)) AS AVG_TotalActiveMinutes,
  CASE
    WHEN (AVG(FairlyActiveMinutes) + AVG(VeryActiveMinutes)) >= 20 THEN 'ACHIEVED'
    ELSE 'NOT ACHIEVED'
  END AS weekly_active_goals
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`
AVG_TotalActiveMinutes
GROUP BY Id
ORDER BY AVG_TotalActiveMinutes DESC
````
For full results: [click here](https://drive.google.com/file/d/1bxA95a0UVpziBAaxMIM3i_fdeI5ab8pm/view)

<img width="390" alt="Screenshot 2024-07-22 at 7 53 50 PM" src="https://github.com/user-attachments/assets/e15524d0-e2ba-4e23-9350-71884d744b4d">


- Adults should aim for 150–300 minutes of moderate-intensity physical activity each week, which translates to about 20-43 minutes daily.
- According to our findings, out of 33 users, 20 were able to achieved an average of 20-117 minutes of fairly to very active minutes each day. This indicates that a significant portion of users are meeting or exceeding the recommended daily physical activity levels.

  
````sql
SELECT 
  Id,
  ROUND(AVG(TotalSteps)) AS AVG_TotalSteps,
  ROUND(AVG(Calories)) AS AVG_Calories,
  ROUND(AVG(TotalDistance)) AS AVG_TotalDistance,
  ROUND((AVG(VeryActiveDistance) / AVG(TotalDistance)) * 100) AS PercentageActiveDistance,
  ROUND(AVG(TotalMinutesAsleep)) AS AVG_TotalMinutesAsleep
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`
GROUP BY Id
ORDER BY PercentageActiveDistance DESC
````

For full results: [click here](https://drive.google.com/file/d/1u-1dE0dAg1qgp80qJ4yhwcLt-Wx0IQ-i/view)

- The Centers for Disease Control and Prevention (CDC) recommends walking at least 10,000 steps per day.
- Based on our findings, out of 33 users, only 7 meet or exceed this daily recommendation, while 6 users record fewer than 4,000 steps per day.
- Notably, these 6 users also have the highest total sleep duration, ranging from 417 to 652 minutes, or approximately 7 to 11 hours per day. Additionally, I noticed a correlation between the number of steps taken and calories burned: users who took around 8,000 steps per day were able to burn at least 2,000 calories or more.
- While the optimal amount of sleep can vary among individuals, the Centers for Disease Control and Prevention (CDC) recommends that adults get at least 7 hours of sleep each night.
- Among the 24 users with sleep records, only half achieved this recommended amount. Additionally, some users recorded significantly lower sleep durations, ranging from just 60 to 70 minutes per day.

````sql
SELECT 
  ActivityDay,
  ROUND(AVG(Calories)) AS AVG_Calories,
  ROUND(AVG(TotalIntensity)) AS AVG_TotalIntensity,
  ROUND(AVG(StepTotal)) AS AVG_StepTotal
FROM (
  SELECT 
    Id,
    ActivityDay,
    ActivityDate,
    SUM(Calories) AS Calories,
    SUM(TotalIntensity) AS TotalIntensity,
    SUM(StepTotal) AS StepTotal
  FROM `omega-terrain-424207-q5.bellabeat.hourly_activity` 
  GROUP BY Id, ActivityDay, ActivityDate
) 
GROUP BY ActivityDay
ORDER BY
  CASE
    WHEN ActivityDay = 'Monday' THEN 1
    WHEN ActivityDay = 'Tuesday' THEN 2
    WHEN ActivityDay = 'Wednesday' THEN 3
    WHEN ActivityDay = 'Thursday' THEN 4
    WHEN ActivityDay = 'Friday' THEN 5
    WHEN ActivityDay = 'Saturday' THEN 6
    WHEN ActivityDay = 'Sunday' THEN 7
  END
````

<img width="635" alt="Screenshot 2024-07-22 at 6 33 46 AM" src="https://github.com/user-attachments/assets/54ae29cc-e181-4c4b-9263-c69d11feea84">


````sql
SELECT 
  Period,
  ROUND(AVG(Calories)) AS AVG_Calories,
  ROUND(AVG(TotalIntensity)) AS AVG_TotalIntensity,
  ROUND(AVG(StepTotal)) AS AVG_StepTotal
FROM (
  SELECT 
    Id,
    Period,
    ActivityDate,
    SUM(Calories) AS Calories,
    SUM(TotalIntensity) AS TotalIntensity,
    SUM(StepTotal) AS StepTotal
  FROM `omega-terrain-424207-q5.bellabeat.hourly_activity` 
  GROUP BY Id, Period, ActivityDate
) 
GROUP BY Period
ORDER BY
  CASE
    WHEN Period = 'Morning' THEN 1
    WHEN Period = 'Afternoon' THEN 2
    WHEN Period = 'Evening' THEN 3
    When Period = 'Night' THEN 4
  END
````
<img width="640" alt="Screenshot 2024-07-22 at 7 12 17 PM" src="https://github.com/user-attachments/assets/1919b618-f180-4903-9770-f99229b4195e">


````sql
SELECT 
  ActivityHour,
  ROUND(AVG(Calories)) AS AVG_Calories,
  ROUND(AVG(TotalIntensity)) AS AVG_TotalIntensity,
  ROUND(AVG(StepTotal)) AS AVG_StepTotal
FROM `omega-terrain-424207-q5.bellabeat.hourly_activity`
GROUP BY ActivityHour
ORDER BY
  CASE
    WHEN ActivityHour = '00:00:00' THEN 1
    WHEN ActivityHour = '01:00:00' THEN 2
    WHEN ActivityHour = '02:00:00' THEN 3
    WHEN ActivityHour = '03:00:00' THEN 4
    WHEN ActivityHour = '04:00:00' THEN 5
    WHEN ActivityHour = '05:00:00' THEN 6
    WHEN ActivityHour = '06:00:00' THEN 7
    WHEN ActivityHour = '07:00:00' THEN 8
    WHEN ActivityHour = '08:00:00' THEN 9
    WHEN ActivityHour = '09:00:00' THEN 10
    WHEN ActivityHour = '10:00:00' THEN 11
    WHEN ActivityHour = '11:00:00' THEN 12
    WHEN ActivityHour = '12:00:00' THEN 13
    WHEN ActivityHour = '13:00:00' THEN 14
    WHEN ActivityHour = '14:00:00' THEN 15
    WHEN ActivityHour = '15:00:00' THEN 16
    WHEN ActivityHour = '16:00:00' THEN 17
    WHEN ActivityHour = '17:00:00' THEN 18
    WHEN ActivityHour = '18:00:00' THEN 19
    WHEN ActivityHour = '19:00:00' THEN 20
    WHEN ActivityHour = '20:00:00' THEN 21
    WHEN ActivityHour = '21:00:00' THEN 22
    WHEN ActivityHour = '22:00:00' THEN 23
    WHEN ActivityHour = '23:00:00' THEN 24
  END
````
<img width="408" alt="Screenshot 2024-07-22 at 8 01 39 PM" src="https://github.com/user-attachments/assets/5b8d9644-1ffb-411b-a729-ad55a5d11002">
