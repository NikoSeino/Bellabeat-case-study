# SUMMARY: Bellabeat Smart Device Data Analysis
#### Completed by: Niko Seino
#### Date: 7/7/2022
### [Tableau Dashboard](https://public.tableau.com/views/BellabeatCaseStudy_16572546536690/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)
```
_Background Information_

**Bellabeat** is a high-tech company that manufactures health-focused smart products. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. 

### ASK
```
_Business Task_

Sršen, the company's cofounder, would like an analysis of Bellabeat’s available consumer data to identify opportunities for growth. She has asked the marketing analytics team to analyze smart device usage data in order to gain insight into how people are already using their smart devices. Then, using this information, she would like recommendations for how these trends can inform Bellabeat marketing strategy. Therefore, in this case study, I answer the following questions:

1. What are some trends in smart device usage?
2. How can these trends help influence Bellabeat marketing strategy?

### PREPARE
```
_About the Data_

The data for this case study comes from [Fitbit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit), a public domain dataset available on Kaggle. It contains personal fitness tracker information from 30 FitBit users. These 30 Fitbit users consented to the submission of all personal tracker data contained in this dataset. 

_Limitations_

- Sample size: 30 people is not a large enough sample to be representative of all FitBit users
- Outdated: The dataset contains data from a one month period in 2016 only. For a deeper and more accurate analysis of trends, we would need data from the current year, preferably collected for an entire year to look at if trends vary during different times of year. 
- Limited: The dataset does not contain any demographic information about the users, including gender, age, or location, which would be beneficial for marketing purposes to target specific customers

### PROCESS
```
1. Install and load packages
```{r}
install.packages('tidyverse')
install.packages('dplyr')
install.packages('ggplot2')
install.packages('lubridate')
library(tidyverse) 
library(dplyr) 
library(ggplot2) 
library(lubridate) 
```
2. Load CSV files containing our data
```{r}
daily_activity <- read.csv("dailyActivity_merged.csv")
intensities <- read.csv("hourlyIntensities_merged.csv")
hourly_steps <- read.csv("hourlySteps_merged.csv")
daily_sleep <- read.csv("sleepDay_merged.csv")
weight <- read.csv("weightLogInfo_merged.csv")
```
3. Check that data imported correctly and review column names
```{r}
head(daily_activity)
head(intensities)
head(hourly_steps)
head(daily_sleep)
head(weight)
```{r}
colnames(daily_activity)
```
4. Identify number of participants in each data set by counting distinct IDs
```{r}
n_distinct(daily_activity$Id)
n_distinct(intensities$Id)
n_distinct(hourly_steps$Id)
n_distinct(daily_sleep$Id)
n_distinct(weight$Id)
```
5. Gain further insights by viewing summary stats of the data sets.

```{r}
#general activity
daily_activity %>%  
  select(TotalSteps,
         Calories,
         TotalDistance,
         SedentaryMinutes) %>%
  summary()
#active minutes
daily_activity %>%  
  select(VeryActiveMinutes, 
         FairlyActiveMinutes, 
         LightlyActiveMinutes) %>%
  summary()
#steps
hourly_steps %>% 
  select(StepTotal) %>% 
  summary()
#sleep
daily_sleep %>% 
  select(TotalMinutesAsleep,
         TotalTimeInBed) %>% 
  summary()
#weight
weight %>% 
  select(WeightPounds,
         Fat,
         BMI) %>% 
  summary()
```

## ANALYZE
Merging the Data
```{r}
combined_data <- merge(daily_sleep, daily_activity, by="Id")
head(combined_data)
```
```{r}
combined_steps <- merge(daily_activity, hourly_steps, by="Id")
head(combined_steps)
```
```{r}
combined_weight <- merge(daily_activity, weight, by="Id")
head(combined_weight)
```
Creating Visualizations
1. What's the relationship between steps taken in a day and sedentary minutes?
```{r}
ggplot(data=daily_activity, aes(x=TotalSteps, y=SedentaryMinutes)) + geom_point() + geom_smooth() + labs(title="Total Steps vs. Sedentary Minutes")
```
There appears to be no significant relationship between total daily steps taken and sedentary minutes. 

2. What's the relationship between minutes asleep and time in bed?
```{r}
ggplot(data=daily_sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + geom_point() + geom_smooth() + labs(title="Time Asleep vs. Time in Bed")
```
As expected, there appears to be a positive linear relationship between total minutes asleep and total time in bed. Participants who spent more time asleep spent more time in bed. 

3. Additional visualizations completed in Tableau. 
These visualizations show the following smart device usage trends:
- Sedentary minutes took up the majority of participants' days and were fairly consistent throughout the week.
- Average "very active minutes" were also consistent throughout the week at around 20 minutes each day.
- On average, participants slept the most on Sundays, which was also the day they look the least amount of steps
- Participants took the most steps on Tuesdays and Saturdays.
- On average, the fewest steps were taken at 3:00 and the most steps taken at 18:00.

## SHARE
[Bellabeat Dashboard - Tableau](https://public.tableau.com/views/BellabeatCaseStudy_16572546536690/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)
<div class='tableauPlaceholder' id='viz1657267313918' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Be&#47;BellabeatCaseStudy_16572546536690&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='BellabeatCaseStudy_16572546536690&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Be&#47;BellabeatCaseStudy_16572546536690&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>     

## ACT
_Recommendations_
How can these trends help influence Bellabeat marketing strategy?

1. Very few customers utilized the weight log feature, so this does not appear to be a selling point. Focus on marketing other features such as activity, sleep, and steps tracking, and consider further research into how to make the weight log feature more user-friendly.

2. Our data shows that when active, participants engaged the most in "light" activity and did not have many "very active" minutes each day. The company could add a "level up" feature in which participants can earn points based on time spent being active, with higher levels of activity earning more points. This could motivate users to engage in active minutes more often.

3. There's about a 1000 step decrease on Sundays compared to the other days of the week. A notification on Sunday mornings with a goal to hit a certain number of steps, along with a reward for hitting a 7-day streak could help close this gap and encourage customers to use the device all days of the week.

4. Based on data showing the most usage around 6pm, it seems likely most users have typical work hours during the day and get most of their steps in after work. A reminder notification around 12pm and 8pm can encourage users to increase their activity levels during other break times such as lunch and after dinner as well. 
