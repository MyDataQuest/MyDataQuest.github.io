---
layout: post
title: "Capstone Project - Data Cleaning"
subtitle: "Walking through my data cleaning process."
date: 2022-10-30 11:12:13 -0400
background: '/img/posts/bikebridge.jpg'
---

<style>
img {border: 2px solid #555;}
</style>


## Data Source
The dataset provided are separate csv files, each representing a month worth’s of historic rides. For the purpose of this analysis, we will be downloading a 12 month range from October 2021 to September 2022. Please click the following [***link***](https://divvy-tripdata.s3.amazonaws.com/index.html) to view the license to use this public dataset. Please click the following [***link***](https://ride.divvybikes.com/data-license-agreement) to view the data license agreement. 

## Pre-Prep 
I Started by opening each of the csv files using MS Excel and adding 7 columns with formulas to the dataset .
The first one being a field to calculate the duration/elapsed time of the ride
- ride_length_mins  = (D2-C2)\*24\*60

The following columns break down the "started_at" / "ended_at" time stamp to a finer granularity which will be useful for future analysis and drilldowns. 

- Ride_date  =TEXT(C2,"mm/dd/yyyy")
- day_of_week  =TEXT(C2,"dddd")
- month  =TEXT(C2,"mmmm")
- year  =TEXT(C2,"yyyy")
- time_started  =TEXT(C2,"HH:MM:SS")
- time_ended  =TEXT(D2,"HH:MM:SS")

## Combining Dataset
Since the files are too large to combine and save in Excel, I leveraged R to Load, consolidate and export the dataset. I made 12 separate dataframes by reading the individual csv files.  

``` R
df1 <- read.csv("df1.csv")
df2 <- read.csv("df2.csv")
df3 <- read.csv("df3.csv")
df4 <- read.csv("df4.csv")
df5 <- read.csv("df5.csv")
df6 <- read.csv("df6.csv")
df7 <- read.csv("df7.csv")
df8 <- read.csv("df8.csv")
df9 <- read.csv("df9.csv")
df10 <- read.csv("df10.csv")
df11 <- read.csv("df11.csv")
df12 <- read.csv("df12.csv")
```
After each dataframe is accounted for, I ran the str(X) function to check the overall structure of each dataframe and made sure it was consistent. 

``` R
str(df1)
str(df2)
str(df3)
str(df4)
str(df5)
...

```
<img src = "/img/posts/Capstone/STR_Check.JPG" alt = "Users Pie Chart" width = "900" class = "center">

After confirming the structural consistency of the 12 dataframes, I merged them into a combined dataframe using the rbind function. 

``` R
combined_df <- rbind(df1, df2, df3, df4, df5, df6, df7, df8, df9, df10, df11, df12)

```

Finally Exported the combined dataset using the write.csv function.

``` R
write.csv(combined_df, "C:/Users/joshu/Documents/Data Analytics Projects/Bikesharing case study/Raw Data/Scrubbed Files/minutes view/combined_dataset.csv", row.names = FALSE) 

``` 

## Data Cleaning in SQL
After uploading the exported CSV file to SQL (Big Query), I wrote some queries to further scrub the data. During the preparation phase, I noticed some blanks in the start_station_id & end_station_id fields. There are also some ride_lengths that are very short and even negative. Negative ride_length would mean that the ended_at time stamp precedes the started_at time stamp which shouldn’t happen. Therefore I wrote a query to specify the following:

- Omit rows with a blank ride_id
- Omit rows with blank start_station_id & end_station_id 
- Only include rows where ride_length is atleast 1 minute
 

``` sql
SELECT *
FROM `cyclistic-bikes-project.Cyclistic_ride_dataset.ride`
where 
  ride_id != "" 
  and ride_length >= '00:01:00' 
  and start_station_id != ""
  and end_station_id != ""

```

## Adding additional columns
The following case statements and subqueries were added to create the "date_type" and "season" columns. These columns would help in our analysis to understand how annual members and casual riders use Cyclistic's rental bikes differently. 

``` sql
SELECT
  ride_id, rideable_type, started_at, ended_at, ride_date, ROUND(ride_length_mins,3) AS ride_length_mins, day_of_week,
  CASE
    WHEN ride_date in (select date from `cyclistic-bikes-project.Cyclistic_ride_dataset.us_holidays`) THEN "Public Holiday"
    WHEN ((select(extract(dayofweek from ride_date))) in(7,1)) and ride_date not in (select date from `cyclistic-bikes-project.Cyclistic_ride_dataset.us_holidays`) THEN "Weekend"
    ELSE "Weekday"
  END AS date_type,
  month, year, 
  CASE
    WHEN ride_date between '2021-09-22' and '2021-12-20' THEN "Fall"
    WHEN ride_date between '2021-12-21' and '2022-03-19' THEN "Winter"
    WHEN ride_date between '2022-03-20' and '2022-06-20' THEN "Spring"
    WHEN ride_date between '2022-06-21' and '2022-09-21' THEN "Summer"
    WHEN ride_date between '2022-09-22' and '2022-12-20' THEN "Fall"
  ELSE "Check"
  END AS season,
  time_started, time_ended, start_station_id, start_station_name, end_station_id, end_station_name, member_casual as member_type, start_lat, start_lng, end_lat, end_lng
FROM `cyclistic-bikes-project.Cyclistic_ride_dataset.cyclistic_ride_cleaned` 
ORDER BY started_at asc

```

The output of the query above is then saved as a final table in BigQuery. This final table is then exported and uploaded to Tableau Public. 

## Export to Tableau

Please use the following [link](https://public.tableau.com/app/profile/joshua.alexander.hasan/viz/Cyclistic2_0/Ride_Dashboard) to view the public tableau dashboard. 



Please follow the link to view the original [Project Prompt](/2022/10/30/Project-Prompt) 

