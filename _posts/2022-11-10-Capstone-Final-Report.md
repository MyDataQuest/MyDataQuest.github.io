---
layout: post
title: "Capstone Project - Final Report"
subtitle: "Final Report with Analysis, Data Viz and Recommendations."
date: 2022-11-10 11:12:13 -0400
background: '/img/posts/bikebridge.jpg'
---

<style>
img {border: 2px solid #555;}
</style>

## Prompt
Cyclistic (a fictional company) is a bike-sharing company based in Chicago that services the city with 5,824 geotracked bicycles available for rent and a network of 692 stations across the city.  The marketing team were asked to design marketing strategies aimed to convert casual riders into annual members. As a junior data analyst in the marketing team, my task is to answer the question **“How do annual members and casual riders use Cyclistic bikes differently?”.** Public data regarding Cyclistic’s rides history was provided. Please use the following [***link***](/2022/10/30/Project-Prompt) to see full project prompt details. 

## Preparation Process
Cyclistic have provided historical trip data in the format of csv files. Each csv file contains a month’s worth of trip data. For the purpose of this case study, data from October 2021 to September 2022 will be analyzed. 
- Leveraged MS Excel for initial analysis and added several calculated columns to drill down time veraibles to a finer granularity.
- Consolidated Raw CSV files using R, due to the file size limitation in MS Excel. 
- Uploaded the consolidated raw data to Big Query where we leveraged SQL queries to:
    - Do some data cleaning.
    - Added columns to the cleaned dataset.
- Saved the final result as a separate table
- Uploaded the final result to Tableau to run some data visualizations. 

Please see the following [***link***](/2022/10/30/Capstone-Cleaning) to see more details about the raw data cleanup. 
 
## Analysis & Visualization

Please use the following [***link***](https://public.tableau.com/app/profile/joshua.alexander.hasan/viz/Cyclistic2_0/Ride_Dashboard) to view the Public Tableau dashboard. 

<img src = "/img/posts/Capstone/Casual_VS_Members_Pie.JPG" alt = "Users Pie Chart" width = "500" class = "center">

After analyzing Cyclistic’s ride history data, we can see that roughly 60% of the rides in the past 12 months were made by annual members. This presents a good growth opportunity for Cyclistic to convert the remaining 40% of their users from casual riders to annual members and increase the reliability of their revenue stream.

<img src = "/img/posts/Capstone/Rides_By_Month_Bar.JPG" alt = "Rides by Month" width = "1350" class = "center">

After breaking down the dataset by months, we see a seasonality pattern in the number of rides taken throughout the year. There is a build up in rides during spring and it peaks in the summer months. Conversely, rides start to drop in the late fall months and have a stark drop during winter. The overall weather and season has a great impact on demand for rides. This makes sense, in a city like Chicago where winters are brutal and summers are lovely. 

<img src = "/img/posts/Capstone/Rides_Map_View.JPG" alt = "Rides by Month" width = "1350" class = "center">

Using the latitude and longitude information from the bike rental stations, a map is charted (above) to show how the casual members use the bike rental service differently geographically to the annual members. We observe how the annual members’ start stations are more spread out across the city, whilst the casual members seem to centralize around the pier and closer to tourist attractions in downtown Chicago. This signals the different use patterns between the 2 membership types. Where leisure use is more prevalant for casual members and commutes are a dominant use amongst annual members.

<img src = "/img/posts/Capstone/Distribution_By_Weekday.JPG" alt = "Rides by Weekday" width = "900" class = "center">


A deeper dive on the weekly usage between the two rider types reveal an interesting pattern. During weekdays, rides taken by annual members heavily outnumber rides by casual users. However, we see the opposite during the weekends. Even though the annual members vs regular members population are split 60 : 40, we see rides taken by casual riders outnumber rides by annual members during weekends.  

<img src = "/img/posts/Capstone/Distribuion_By_Time.JPG" alt = "Rides by Hour of Day" width = "900" class = "center">

The chart above takes a closer look at both class of user's rides broken down to different hours of the day to analyze rider's patterns. Overall there is a spike in usage around 7-8am and another spike around 4-5pm amongst all users which are popular commute times, going in and out of work. From 4am – 9pm, the annual members usage are higher than casual riders. Conversely, from 10pm -3am, the casual users have more rides than the annual members. 

<img src = "/img/posts/Capstone/Rides_By_Date_Type.JPG" alt = "Rides by Hour of Day" width = "900" class = "center">

A "Date Type" column was added to the data table by referencing a list of US public holidays in order to determine if a date falls on a "Weekday", "Weekend" or a "Public Holiday". Analyzing the rider's usage throughout the year based on the date type reveals that public holidays and weekends account for a larger demand in rides amongst casual users totaling 43% of its rides. Whilst there is less of an impact amongst member users, where weekends and public holidays only account for 27.8% of its ridership and a greater portion of rides are taken during the weekdays. 

Several observations were made after analyzing the rides dataset from a time and geographical standpoint: 
- Annual members use are more regimented with peaks during popular commute times in the morning and in the evening. And overall ride demand peaks in the middle of the work week. 
- Casual members useage are dominantly for leisure, where the peak of rentals happen after work hours and ride demands are heavily boosted during weekends and public holidays. 
- Both memberships share a seasonality demand cycle    

## Recommendations

- Since we identified seasonal demand in bicycle rides throughout the year, Cyclistic needs to intensify their digital marketing campaign via social media or emails during the Spring season, leading up to the summer months. Having promotions or markdown sales for memberships towards the end of the summer months might also attract more casual members and lock in an annual membership contract for the following year. 

- Furthermore, Cyclistic could also launch marketing campaigns directed at casual members to take advantage of Cyclistic's annual membership and to view bicycle rides as more than a weekend past time but also a good alternative transport to work. Reminding them of the benefits of exercise and pottential commute times saved by taking short bike rides instead of traffic jams or congested public transportation.  

- Sending email blasts and social media posts to also reminders casual members on upcoming long weekend or public holiday and inviting them to be an annual member

## Future Analysis

- It would be nice to also have pricing and billing information along with unique user identifiers such as an email address or user_id for casual members. This information could be used to calculate their average monthly/weekly spending on cyclistic bikes and show how much they could be saving by having a yearly membership instead. However, it is understandable that this information/data is not made available in the public dataset. 