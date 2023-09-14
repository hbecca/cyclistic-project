# Cyclistic_Analysis
![Cycle](https://images.pexels.com/photos/3632581/pexels-photo-3632581.jpeg)
## Introduction
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geo-tracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. 
One approach that helped Cyclistic marketing team in building general awareness was by creating flexibility of pricing plans: Single-ride passes, full-day passes and annual membership. Customers who purchase single-ride or full-day passes are referred to as casual riders while customers who purchase annual memberships are Cyclistic members.

**_Disclaimer_**: This is my google data analytics programme project.

## Problem statement
How do annual members and casual riders use Cyclistic bikes differently?

## Skills/Tools used:
-  Excel
- SQL
- Power BI

## Dataset
The dataset was downloaded from [here](https://divvy-tripdata.s3.amazonaws.com/index.html). I downloaded the 2022(Jan-Dec) year dataset. The data has been made available by Motivate International Inc.

## Cleaning Steps
1. Downloaded the Cyclistic trip data.
2. Created a folder on my desktop and moved my files there.
3. Opened my spreadsheet and created a column which i subtracted column "ended_at" from column "started_at" and named it column "ride_length" using HH:MM:SS as the format.
4. Created a column and called it "day_of_week" and calculated it by using the WEEKDAY command on excel

## Analysis
**Step 1** -**Importing Dataset** 
This was a bit of challenge for as i kept getting error messages but i was able to figure it out. I have now understood how to import flat files to SQL

**Step 2** -**Joining Tables**

```Select * into cyclistic_table from January
Union all
Select * from February
Union all
Select * from March
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, cast(start_station_id as nvarchar), 
end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng, member_casual from April
Union all
Select * from May
Union all
Select * from June
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, cast(start_station_id as nvarchar), 
end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng, member_casual from July
Union all
Select * from August
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, start_station_id, 
end_station_name, cast(end_station_id as nvarchar), start_lat, start_lng, end_lat, end_lng, member_casual from September
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, cast(start_station_id as nvarchar), 
end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng, member_casual from October
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, cast(start_station_id as nvarchar), 
end_station_name, cast(end_station_id as nvarchar), start_lat, start_lng, end_lat, end_lng, member_casual from November
Union all
Select ride_id, rideable_type, day_of_week, started_at, ended_at, ride_length, start_station_name, start_station_id, 
end_station_name, cast(end_station_id as nvarchar), start_lat, start_lng, end_lat, end_lng, member_casual from December
```

I ran the above code to join the tables January-December.
As you can see from the code above, I had to write out the columns for April, July, September, October, November, and December because, in contrast to the others, their "started_at" and "ended_at" columns are of the float data type rather than the Nvarchar data type. This change was necessary to allow for joining.

**Step 3** -**Calculating Ride Statistics**
Firstly i calulated the Total number of rides taken for the year 2022 using the COUNT Function and then I calculated the Total Rides and Average ride of each rider type ie "member" and "casual" Using WHEN, COUNT, DATEDIFF and GROUPBY Functions with this code chunks:

```SELECT COUNT(ride_id) AS total_ride 
   FROM cyclistic_table

SELECT
    CASE
        WHEN member_casual = 'member' THEN 'Member'
        WHEN member_casual = 'casual' THEN 'Casual'
        ELSE 'Other'
    END AS rider_type,
    COUNT(ride_id) AS total_rides,
    AVG(DATEDIFF(MINUTE, started_at, ended_at)) AS avg_ride_duration
FROM
    cyclistic_table
Group by
member_casual
```

I also calculated the Total number of time taken(minutes) by each Rider type using DATEDIFF and GROUPBY functions:
```SELECT member_casual, 
   SUM(DATEDIFF(MINUTE, started_at, ended_at)) As Total_minutes
   FROM cyclistic_table
   GROUP BY member_casual
```

**Step 5** -**More Analysis**
To establish which weekdays had more or fewer rides, I created smaller tables that displayed the number of rides taken by Members and Casual Riders.
```SELECT DATENAME(WEEKDAY,started_at) AS week_day,
   COUNT(*) AS ride_count
   FROM
   cyclistic_table
   WHERE
   member_casual = 'casual'
   GROUP BY
   DATENAME(WEEKDAY,started_at)
   ORDER BY
    Case
	When DATENAME(WEEKDAY,started_at) = 'Sunday' Then 1
	When DATENAME(WEEKDAY,started_at) = 'Monday'Then 2
	When DATENAME(WEEKDAY,started_at) = 'Tuesday'Then 3
	When DATENAME(WEEKDAY,started_at) = 'Wednesday'Then 4
	When DATENAME(WEEKDAY,started_at) = 'Thursday'Then 5
	When DATENAME(WEEKDAY,started_at) = 'Friday'Then 6
	When DATENAME(WEEKDAY,started_at) = 'Saturday'Then 7
	END
    
   SELECT DATENAME(WEEKDAY,started_at) AS week_day,
   COUNT(*) AS ride_count
   FROM
   cyclistic_table
   WHERE
   member_casual = 'member'
   GROUP BY
   DATENAME(WEEKDAY,started_at)
   ORDER BY
    Case
	When DATENAME(WEEKDAY,started_at) = 'Sunday' Then 1
	When DATENAME(WEEKDAY,started_at) = 'Monday'Then 2
	When DATENAME(WEEKDAY,started_at) = 'Tuesday'Then 3
	When DATENAME(WEEKDAY,started_at) = 'Wednesday'Then 4
	When DATENAME(WEEKDAY,started_at) = 'Thursday'Then 5
	When DATENAME(WEEKDAY,started_at) = 'Friday'Then 6
	When DATENAME(WEEKDAY,started_at) = 'Saturday'Then 7
	END
```

