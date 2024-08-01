# 👩🏻‍💻 Data Visualization with Tableau

- In this data visualization created using Tableau for the Bellabeat case study, we explore the relationship between lifestyle metrics and wellness outcomes. The visualizations reveal key insights into how variables like total steps and sleep duration interact, addressing our original question about their impact on overall wellness.

- Our target audience includes health and wellness professionals, marketing team, and stakeholders at Bellabeat. To effectively communicate with them, we have created clear, engaging visualizations that highlight the main insights and trends. By using Tableau's advanced visualization tools, we make complex data more accessible and understandable.

## 📚 Correlation Visualization 

### 🏃🏻‍♀️ Total Minutes Asleep and Total Steps

<img width="850" alt="Screenshot 2024-07-30 at 3 20 47 AM" src="https://github.com/user-attachments/assets/a918c9ea-4677-4b40-b6e0-3b0d0abfb158">

- It shows a positive correlation between total minutes asleep and total steps, though the relationship is weak. 
- The R-squared value of 0.047 indicates that only about 4.7% of the variation in sleep duration can be explained by the number of steps taken.
- Additionally, the p-value of 0.307 suggests that this correlation is not statistically significant, meaning there is a high probability that the observed relationship is due to random chance rather than a meaningful connection.
- Overall, while there is a slight positive trend, the evidence does not strongly support a significant impact of step count on sleep duration.

### 🏃🏻‍♀️ Burned Calories and Total Steps

<img width="788" alt="Screenshot 2024-07-30 at 3 21 11 AM" src="https://github.com/user-attachments/assets/5d4d1d2c-8f43-4899-afb3-9681331d9cfe">

- It shows a positive correlation between calories burned and total steps taken.
- The R-squared value of 0.192 indicates that approximately 19.2% of the variation in calories burned can be explained by the number of steps taken.
- The P-value of 0.010 suggests that this correlation is statistically significant, meaning there is a strong likelihood that the relationship observed is meaningful and not due to random chance.
- This indicates that as the number of steps increases, the calories burned also tend to increase, reflecting a meaningful connection between physical activity and energy expenditure.


### 🏃🏻‍♀️ Activity Levels and Burned Calories

<img width="757" alt="Screenshot 2024-07-30 at 3 24 37 AM" src="https://github.com/user-attachments/assets/ef57e204-eec0-4c1a-9dd8-4a6e693c6423">

✏️ Calories vs. Sedentary Minutes
- The R-squared value of 0.684 indicates that about 68.4% of the variation in calories burned can be explained by the amount of sedentary time.
- The P-value of less than 0.0001 suggests this correlation is highly statistically significant, meaning it is very unlikely that the relationship observed is due to random chance.
- This indicates that as sedentary minutes increase, calories burned significantly decrease, underscoring the impact of physical inactivity on lower energy expenditure.

✏️ Calories vs. Fairly Active Minutes
- It shows a positive correlation between calories burned and fairly active minutes.
- The R-squared value of 0.2863 indicates that approximately 28.63% of the variation in calories burned can be explained by the amount of fairly active minutes.
- The p-value of 0.0016 suggests that this correlation is statistically significant, meaning there is a strong likelihood that the observed relationship is meaningful and not due to random chance.
- This indicates that as fairly active minutes increase, the calories burned also tend to increase, reflecting a significant connection between moderate physical activity and energy expenditure.

✏️ Calories vs. Lighty Active Minutes
- It indicates a positive correlation between calories burned and lightly active minutes.
- The R-squared value of 0.60636 suggests that approximately 60.64% of the variation in calories burned can be explained by the amount of lightly active minutes.
- The P-value of less than 0.0001 indicates that this correlation is highly statistically significant, meaning the observed relationship is very unlikely to be due to random chance.
- This strong positive correlation demonstrates that as lightly active minutes increase, calories burned also significantly increase, highlighting the impact of light physical activity on energy expenditure.

✏️ Calories vs. Very Active Minutes
-It shows a strong positive correlation between calories burned and very active minutes. 
- The R-squared value of 0.729807 indicates that approximately 72.98% of the variation in calories burned can be explained by the amount of very active minutes.
- The P-value of less than 0.0001 suggests that this correlation is highly statistically significant, meaning the relationship observed is very unlikely to be due to random chance.
- This strong positive correlation demonstrates that as very active minutes increase, calories burned also significantly increase, underscoring the substantial impact of high-intensity physical activity on energy expenditure.


## 🏃🏻‍♀️ Daily Steps Analysis

The daily step count per user visualization categorizes users based on their daily step counts into four distinct activity levels: Very Active, Physically Active, Physically Inactive, and Sedentary. Here are the key insights:

🔺 Very Active Users: Users with step counts greater than 13,000 steps per day fall into this category. The highest average daily steps recorded is 16,040. This group demonstrates high levels of physical activity, which likely contributes positively to their overall health and well-being.

🔺 Physically Active Users: Their step counts range from 10,776 to 12,417. This group maintains a healthy level of daily activity, meeting or exceeding common fitness recommendations.

🔺 Physically Inactive Users: The step counts range from 6,482 to 9,753, indicating moderate activity levels. These users are active but might benefit from increasing their daily step count to enhance their fitness and health outcomes.

🔺 Sedentary Users: The lowest recorded average daily step count is 916. This group shows very low levels of physical activity, which can have negative implications for their health, including increased risks of chronic diseases.

<img width="847" alt="Screenshot 2024-07-31 at 7 59 51 PM" src="https://github.com/user-attachments/assets/7ddd1f33-eac1-415a-81d9-b683522741cd">

#### Summary
🔹 Activity Distribution: The visualization shows a significant variance in daily step counts across users, indicating diverse activity levels within the user base.

🔹 Health Implications: Users with higher daily step counts (Very Active and Physically Active) are likely to experience better health outcomes. Conversely, those in the Sedentary category might face health challenges due to insufficient physical activity.

#### ✅ Recommendations

Encouraging users in the Physically Inactive and Sedentary categories to increase their daily step counts could lead to improved health and wellness. Targeted interventions and motivational strategies could be developed to help these users achieve higher activity levels.

This analysis provides valuable insights for Bellabeat in understanding the activity patterns of their users and tailoring their products and services to promote higher levels of physical activity.

## 🏃🏻‍♀️ Average Steps Throughout the Week

<img width="843" alt="Screenshot 2024-07-30 at 3 22 57 AM" src="https://github.com/user-attachments/assets/11b1746e-9ed0-4025-93d6-6815aff12bd9">

The average steps per day analysis provides valuable insights into the weekly activity patterns of Bellabeat users. Here are the key observations:

🔹  Highest Activity Day: Saturday is the most active day of the week, with an average step count of 8,162. This suggests that users are most likely to engage in physical activities and walking during weekends, possibly due to more leisure time.

🔹 Lowest Activity Day: Sunday has the lowest average step count at 6,899. This indicates a significant drop in physical activity compared to other days, likely due to it being a day of rest or lower engagement in activities.

🔹 Weekday Patterns: Tuesday shows the highest average steps during weekdays, with 8,030 steps, indicating a peak in mid-week activity levels. Monday, Wednesday, Thursday, and Friday have relatively similar step counts, ranging from 7,290 to 7,689 steps, suggesting consistent activity levels throughout the typical workweek.

#### ✅ Recommendations
🔹 Encourage Weekend Activities: Given the high activity levels on Saturdays, Bellabeat could promote weekend challenges or group activities to maintain or increase engagement.

🔹 Boost Sunday Activity: To address the drop in activity on Sundays, Bellabeat could introduce gentle activity prompts, such as family walks or relaxation exercises, to encourage users to stay active even on rest days.

🔹 Maintain Weekday Consistency: The consistent weekday activity levels suggest that users have a stable routine. Bellabeat can leverage this by integrating activity reminders and motivational tips into their daily schedules.

This analysis provides actionable insights for Bellabeat to tailor their programs and features, encouraging users to maintain a balanced and active lifestyle throughout the week.

## 🏃🏻‍♀️ Hourly Steps Analysis

<img width="851" alt="Screenshot 2024-07-31 at 8 27 35 PM" src="https://github.com/user-attachments/assets/ca1a8434-c4e1-4155-9a45-12d93bd93235">

The hourly step count analysis provides a detailed view of the daily activity patterns of Bellabeat users. Here are the key observations:

🔹 Peak Activity Hours: The highest average steps are recorded at 6 PM, with a maximum step total of 599. This suggests that users are most active in the early evening, possibly after work or during evening exercise routines.

🔹 Morning Activity: Activity begins to increase significantly from 6 AM onwards, reaching a noticeable peak at 8 AM with 482 steps, indicating that many users start their day with physical activity, possibly through morning walks or commutes.

🔹 Midday Consistency: From 11 AM to 2 PM, users maintain a relatively high and consistent level of activity, with step counts ranging from 497 to 550 steps. This could reflect a mix of work-related movements and lunch breaks.

🔹 Afternoon Activity: There is a slight dip at 3 PM with 406 steps, followed by a gradual increase again towards the evening peak at 6 PM.

🔹 Evening Decline: After the peak at 6 PM, activity levels start to decline, with a sharp drop by 8 PM (354 steps) and further decrease towards the night.

#### ✅ Recommendations

🔹 Morning Initiatives: Morning fitness programs or reminders could be beneficial to encourage users to start their day with activity.

🔹 Nighttime Relaxation: As activity declines in the evening, Bellabeat could introduce relaxation or winding-down routines to help users transition to rest.

This analysis provides crucial insights into the hourly activity patterns of users, helping Bellabeat tailor their programs and features to fit the natural rhythms of their user base, thereby promoting a more active and balanced lifestyle.

## 🏃🏻‍♀️ Hourly Calories Analysis

<img width="848" alt="Screenshot 2024-07-31 at 8 36 58 PM" src="https://github.com/user-attachments/assets/8dbb47dd-f0ad-4755-b4b3-05c28a243c7c">

🔹 The lowest average burned calories occur between 3 AM and 4 AM, with an average of 68 calories. This aligns with typical sleeping hours.

🔹 The highest average burned calories are recorded between 5 PM and 6 PM, peaking at 123 calories. This likely corresponds to evening workouts or high-energy activities.

🔹 There is a significant increase in activity starting at 5 AM, with calories burned rising from 82 to 103 by 7 AM, indicating an active morning routine.

#### ✅ Recommendations

🔹 Optimize Morning Workouts: Since activity levels start increasing around 5 AM, encourage incorporating morning workouts between 5 AM and 7 AM to capitalize on natural activity peaks.

🔹 Increase Daytime Movement: To avoid the mid-day lull, suggest taking short breaks for stretching or brief walks to maintain a higher calorie burn throughout the day.

🔹 Maximize Evening Activity: Given the high activity levels at 5-6 PM, recommend scheduling high-intensity workouts or physical activities during this period for maximum benefit.

🔹 Monitor Evening Routine: Advise users to engage in light physical activities, such as a walk or yoga, in the evening to maintain a healthy activity level before bedtime without disrupting sleep patterns.

🔹 Consistent Sleep Schedule: Ensure a consistent sleep schedule, aligning with the low activity period of 3-4 AM, to promote recovery and readiness for morning activities.

## ▶️ LAST PHASE: ACT
![hqdefault](https://github.com/user-attachments/assets/46862809-f17e-4ca8-8f2f-0304d65d1cdc)


✅ In the ACT phase of the Bellabeat case study, we leverage insights derived from our comprehensive data analysis to arrive at actionable conclusions. Our objective is to provide stakeholders with data-driven recommendations that address the business problem and facilitate informed decision-making. Here are the recommendations based on the analysis:

🔸 Enhance Weekend Engagement

Saturday Programs: Introduce special fitness challenges or community events on Saturdays to capitalize on high activity levels.
Sunday Initiatives: Launch gentle activity programs or wellness content to boost engagement and activity on Sundays.

🔸 Promote Daily Activity

Morning Routines: Encourage users to incorporate physical activity into their morning routines through reminders and motivational content.
Evening Workouts: Develop evening workout programs or virtual classes to align with peak activity hours at 6 PM.

🔸 Target Sedentary Users

Step Challenges: Implement step challenges with rewards to motivate sedentary users to increase their daily step count.
Personalized Coaching: Provide personalized coaching and activity recommendations for users in the Sedentary and Physically Inactive categories.

🔸 Maintain Consistency

Daily Reminders: Utilize app notifications to remind users to stay active throughout the day, especially during consistent activity periods like late morning and early afternoon.
Midday Breaks: Encourage users to take short activity breaks during work hours to maintain steady activity levels.

🔸 User Engagement and Retention

Feedback Loops: Continuously gather user feedback to refine and improve fitness programs and features.
Community Building: Foster a sense of community through social features, group challenges, and user interaction within the app.

By acting on these recommendations, Bellabeat can enhance user engagement, promote healthier lifestyles, and ultimately drive better business outcomes. Our data-driven approach ensures that the decisions are grounded in real user behavior and trends, leading to more effective and impactful strategies.

------------

Thank you for taking the time to read my Bellabeat case study. I hope the insights and recommendations provided will contribute to making informed, data-driven decisions for a healthier, more active community. Your support and feedback are greatly appreciated! 🌟



