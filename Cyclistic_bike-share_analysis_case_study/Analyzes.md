---

### Scenario:

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director
of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore,
your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights,
your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives
must approve your recommendations, so they must be backed up with compelling data insights and professional data
visualizations.

---

### Ask
##### Guiding Questions
* What is the problem you are trying to solve?

    The goal is to identify differences in how casual riders and annual members use Cyclistic's bikes in hopes of getting casual riders to become members.
    
* How can your insights drive business decisions?
    
    These insights can be used to increase the number of annual memberships furthering the company's success.

---

### Prepare
The data has been uploaded to this folder. The data is ROCCC (reliable, original, comprehensive, current, cited). This is a practice dataset from google 
making it credibile and the data is limited to times and locations so bias doesn't play much of a role. 
However keeping the overall goal in mind, the data doesn't take into account things like rider's financial needs 
which would play a large role in increasing memberships. 
On the other hand, this data can provide key insights into where and for how long the riders use the bikes 
which can lead to key differences in annual members vs casual riders. 

The data contains the following information:

|  Field Name | Type|
| -------- | ------| 
| ride_id | STRING |
| rideable_type | STRING |
| started_at | TIMESTAMP |
| ended_at | TIMESTAMP |
| start_station_name | STRING |
| start_station_id | STRING |
| end_station_name | STRING |
| end_station_id | STRING |
| start_lat | FLOAT |
| start_lng | FLOAT |
| end_lat | FLOAT |
| end_lng | FLOAT |
| member_casual | STRING |


---

### Process
All data was uploaded onto BigQuery. After inspecting the data there were no duplicates, missing data or structural errors like typos or mislabled categories. However, after calulationg the duration for each trip there was a large amount of trips with very short durations. So to only keep serious riders any time durations under 5 minutes have been droped. 

Next, I combinded the data to form the following table:

| Coulman Name | Description |
| ------------ | ----------- |
| member_casual | Is rider a member or casual rider|
| started_at | Date and Time trip began|
| ended_at   | Date and Time trip ended |
| trip_duration_minutes | Duration of trip in minutes|
| trip_duration_days   | Duration of trip in days
| day_of_week | Day of the start of the trip|

In SQL:

```sql
SELECT *
FROM 
    (
        SELECT 
            member_casual,

            started_at,

            ended_at,

            DATETIME_DIFF(ended_at, started_at, MINUTE) as trip_duration_minutes,

            DATETIME_DIFF( ended_at, started_at ,DAY) as trip_duration_days,

            EXTRACT(DAYOFWEEK FROM started_at ) as day_of_week,

        FROM `learned-nimbus-333802.Cyclistic.Cyclistic`
    )

WHERE trip_duration_minutes > 5
```

---

### Analyze 

