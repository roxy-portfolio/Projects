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
- Created new tables: ActivityDay, ActivityDate, ActivityHour and Period 
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

## Data Analysis
I collect and analyze the minimum, maximum, and average values of numeric columns to gain insights into the data and identify any potential outliers that could influence the analysis.

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

### How much physical activity is recommended?

CHILDREN & ADOLESCENTS (5-17 years old)

It is recommended that:
- Children and adolescents should do at least an average of 60 minutes per day of moderateto vigorous-intensity, mostly aerobic, physical
activity, across the week. 
- Vigorous-intensity aerobic activities, as well as those that strengthen muscle and bone,should be incorporated at least 3 days a week.
- Children and adolescents should limit the amount of time spent being sedentary, particularly the amount of recreational screen time. 

ADULTS (18-64 years old)

It is recommended that:
- Adults should do at least 150 – 300 minutes of moderate-intensity aerobic physical activity; or at least 75–150 minutes of vigorousintensity aerobic physical activity; or an equivalent combination of moderate- and vigorous-intensity activity throughout the week,
for substantial health benefits.

OLDER ADULTS (65 years old & older)

It is recommended that:
- Older adults should do at least 150–300 minutes of moderate-intensity aerobic physical activity; or at least 75–150 minutes of vigorousintensity aerobic physical activity; or an equivalent combination of moderate- and vigorous-intensity activity throughout the week, for substantial health benefits.

Source: [World Health Organization](https://www.who.int/publications/i/item/97892400151280)
![images](https://cdn.who.int/media/images/default-source/health-topics/physical-activity/summary-infographic-guideline-on-physical-activity.jpg?sfvrsn=246f54b7_9)

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
