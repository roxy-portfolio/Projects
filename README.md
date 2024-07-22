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

I observed an error occurring whenever I imported certain CSV files due to data type issues. To prevent these errors, I opened the CSV files in Google Sheet and changed the activity_date format from TIMESTAMP to DATE.

![image](https://drive.google.com/file/d/1ZSQj4QQXzExjwcqeGrdALHgqN0pj_ysP/view?usp=drive_link)

-- Checking number of user IDs recorded under daily_activity without duplicates (Total User IDs: 35) -- 
SELECT
 DISTINCT Id
FROM `omega-terrain-424207-q5.bellabeat.daily_activity`

-- Checking number of user IDs recorded under daily_intensities without duplicates (Total User IDs: 33) -- 
SELECT
 DISTINCT Id
FROM `omega-terrain-424207-q5.bellabeat.daily_intensities

-- Checking number of user IDs recorded under sleep_day without duplicates (Total User IDs: 24) -- 
SELECT
 DISTINCT Id
FROM `omega-terrain-424207-q5.bellabeat.sleep_day`

-- Checking number of user IDs recorded under steps without duplicates (Total User IDs: 33) -- 
SELECT
 DISTINCT Id
FROM `omega-terrain-424207-q5.bellabeat.steps`

