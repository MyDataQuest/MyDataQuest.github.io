---
layout: post
title: "My Second Markdown post"
subtitle: "Checking Dev environment from MBP."
date: 2022-10-31 13:28:13 -0400
background: '/img/posts/01.jpg'
---


## Testing first title
Hello this is my second markdown post 

## Dataset preparation
After downloading all the raw data and making the adjustments in Excel, We need to load them to R to combine. 

## Combining and Exporting in R
Loading data to R (Use R markdown file to show reading of the individual excel file as a different dataframe)

``` R
df1 <- read.csv("df1.csv")
df2 <- read.csv("df2.csv")
df3 <- read.csv("df3.csv")
df4 <- read.csv("df4.csv")
df5 <- read.csv("df5.csv")

```
Checking the Structure for consistency using the str(X) function

``` R
str(df1)
str(df2)
str(df3)
str(df4)
str(df5)

```
Merging 5 dataframes into one dataframe using the rbind 

``` R
combined_df <- rbind(df1, df2, df3, df4, df5)

```

Writing out an export file using write.csv

``` R
write.csv(combined_df, "C:/Users/joshu/Documents/Data Analytics Projects/Bikesharing case study/Raw Data/Scrubbed Files/minutes view/combined_dataset.csv", row.names = FALSE) 

``` 

## Data Cleaning in SQL
After uploading the exported CSV file to SQL (Big Query), I used the following query to eliminate null ride_id, station ids and any rides that are shorter than a minute

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
We then add additional date type and season columns by writing a query with cases and subqueries

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

## Export to Tableau

This is then exported to tableau
