# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback. 
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)

  * 983,648 trips

```sql
SELECT count(*) as trips_count
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

- What is the earliest start date and time and latest end date and time for a trip?

  * Earliest: 2013-08-29 09:08:00 UTC
  * Latest: 2016-08-31 23:48:00 UTC

```sql
SELECT min(start_date) AS Earliest, max(end_date) as Latest
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

- How many bikes are there?
  * 700 bikes

```sql
SELECT count(distinct bike_number) as bikes_count
FROM `bigquery-public-data.san_francisco.bikeshare_trips
```


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: Which station had the highest number of bikes on average?
  * Answer: Station 90-  16.64 bikes on average

  * SQL query:

```sql
SELECT station_id, avg(bikes_available) as avg_bikes
FROM `bigquery-public-data.san_francisco.bikeshare_status`
GROUP BY station_id
```
```sql
SELECT *
FROM hanan-w205.Project_1_station_temp.Project_1_station_temp
WHERE avg_bikes = (
    SELECT max(avg_bikes) FROM hanan-w205.Project_1_station_temp.Project_1_station_temp
)
```

- Question 2: Which city has the most stations and which city has the least stations?
  * Answer: Most stations- San Francisco (37), Least stations- Palo Alto (5)

  * SQL query:

```sql
SELECT landmark as city, count(distinct station_id) as number_of_stations
FROM `bigquery-public-data.san_francisco.bikeshare_stations`
GROUP BY landmark
ORDER BY count(distinct station_id) DESC
```

- Question 3: What is the average trip duration?
  * Answer: 16.98 Mins

  * SQL query: 

```sql
SELECT ROUND(avg(duration_sec)/60,2) as avg_trip_duration_in_mins
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:

```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

```sql
# Question 1- "bonus_1 refers to the temporary table"
SELECT start_station_id, end_station_id, region_id, region_name, Count, region_rank
FROM (
    SELECT start_station_id, end_station_id, region_id, region_name, Count, row_number() over (partition by region_id order by Count desc) as region_rank
    FROM (
        SELECT `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id, 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.end_station_id, `hanan-w205.Project_1_station_temp.bonus_1`.region_id, 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.name as region_name, count (*) as Count,
        FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
        INNER JOIN `hanan-w205.Project_1_station_temp.bonus_1` ON `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id = `hanan-w205.Project_1_station_temp.bonus_1`.station_id
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions` ON `hanan-w205.Project_1_station_temp.bonus_1`.region_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.region_id
        GROUP BY `hanan-w205.Project_1_station_temp.bonus_1`.region_id,`bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.name ,`bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id, `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.end_station_id
    )
)
WHERE region_rank <= 5
ORDER BY region_id, Count Desc
```

- Top 3 most popular regions(stations belong within 1 region)
```sql
# Question 2
SELECT region_id, region_name, Count, region_rank
FROM (
    SELECT region_id, region_name, Count, row_number() over (order by Count desc) as region_rank
        FROM (
        SELECT `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.name AS region_name,
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id, count(`bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.trip_id) as Count
        FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` ON
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.station_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions` ON 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.region_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id
        GROUP BY region_id, region_name
        )
    )   
WHERE region_rank <= 3
ORDER BY Count Desc
```


- Total trips for each short station name in each region
```sql
# Question 3
SELECT region_id, region_name, short_name, Count
FROM (
    SELECT region_id, region_name, short_name, Count
        FROM (
        SELECT `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.name AS region_name,
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id, count(`bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.trip_id) as Count,
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.short_name
        FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` ON
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.station_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions` ON 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.region_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id
        GROUP BY region_id, region_name, short_name
        )
    )   
ORDER BY region_name, Count Desc
```


- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.
```sql
# bonus_4_temp temporary view
    SELECT region_id, region_name, Count, region_rank
    FROM (
        SELECT region_id, region_name, region_rank, Count
            FROM (
            SELECT `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.name AS region_name,
                `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id, count(`bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.trip_id) as Count,
                row_number() over (order by count(`bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.trip_id) desc) as region_rank
            FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
            INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` ON
                `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.station_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id
            INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions` ON 
                `bigquery-public-data.san_francisco_bikeshare.bikeshare_regions`.region_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id
            GROUP BY region_id, region_name
            )
        )   
    WHERE region_rank <= 3
    ORDER BY region_rank
```

```sql
# Question 4
SELECT region_id, region_name, bike_number, Count, bike_rank
FROM
    (
    SELECT region_id, region_name, bike_number, Count, row_number() over (partition by region_id order by Count desc) as bike_rank, region_rank
    FROM (
        SELECT `hanan-w205.Project_1_station_temp.bonus_4_temp`.region_id, `hanan-w205.Project_1_station_temp.bonus_4_temp`.region_name, `hanan-w205.Project_1_station_temp.bonus_4_temp`.region_rank,
        `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.bike_number, count(trip_id) as Count
        FROM
        `hanan-w205.Project_1_station_temp.bonus_4_temp`
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info` ON 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.region_id = `hanan-w205.Project_1_station_temp.bonus_4_temp`.region_id
        INNER JOIN `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips` ON 
            `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`.station_id = `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`.start_station_id
        GROUP BY region_id, region_name, region_rank, bike_number
        )
    )
WHERE bike_rank <= 10
ORDER BY region_rank, bike_rank
```

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
     
     ```
     bq query --use_legacy_sql=false 
         'SELECT count(*) as trips_count
         FROM 
                `bigquery-public-data.san_francisco.bikeshare_trips`'
     ```
| trips_count:  |
|:-:|
| 983,648  |

  * What is the earliest start time and latest end time for a trip?

    ```
        bq query --use_legacy_sql=false 
        'SELECT min(start_date) AS Earliest, max(end_date) as Latest
        FROM 
            `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

| Earliest  | Latest  |
|:-:|:-:|
| 2013-08-29 09:08:00  | 2016-08-31 23:48:00  |

  * How many bikes are there?
  
  ```
        bq query --use_legacy_sql=false 
        'SELECT count(distinct bike_number) as bikes_count
        FROM 
            `bigquery-public-data.san_francisco.bikeshare_trips`'
  ```

| bikes_count  |
|:-:|
| 700 |

2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?

```
    bq query --use_legacy_sql=false 
    'SELECT sum(trip_count) as Number_of_trips,
        CASE
            WHEN HOUR < 12 AND HOUR >= 0  THEN "Morning"
            WHEN HOUR >= 12 AND HOUR <= 23  THEN "Afternoon"
            ELSE "Error"
        END as Time_of_day
    FROM (
        SELECT EXTRACT(hour FROM `bigquery-public-data.san_francisco.bikeshare_trips`.start_date) as HOUR, count(*) as trip_count
        FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        GROUP BY HOUR
        ORDER by trip_count
        )
    GROUP BY Time_of_day'
```

| Number_of_trips  | Time_of_day  |
|:-:|:-:|
| 571,309  | Afternoon  |
| 412,339  | Morning  |

### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: Could we use create a "fingerprint" of users, to connect the trip to work and from work for commuters? This will help distinguish commuter trips from non-commuter trips

- Question 2: On which days do people use the service more? And on which days do they use it less?

- Question 3: How many of the commuters are subscribed the the service, vs. non-subscribers? And how many non-commuters?

- Question 4: Which stations are commuters typically using? Are many of them coming/going to the caltrain/ferry?

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: Could we use create a "fingerprint" of users, to connect the trip to work and from work for commuters? This will help distinguish commuter trips from non-commuter trips
  * Answer: Yes, it seems like that is possible to some degree (more details and plot in the Jupyter notebook)
  * SQL query:
  ```sql
      SELECT *
    FROM (
        SELECT *, COUNT(*) over (partition by morning_trip_id) as Morning_ID_Count
        FROM (
            SELECT T1.trip_id as morning_trip_id, T1.duration_sec as morning_duration_sec, T1.start_date as morning_start_date, T1.start_station_name as morning_start_name,
                T1.start_station_id as morning_start_id, T1.end_date as morning_end_date, T1.end_station_name as morning_end_name, T1.end_station_id as morning_end_id,
                T1.bike_number as morning_bike_number, T1.zip_code as morning_zip_code, T1.subscriber_type as morning_subscriber_type, T1.HOUR as morning_hour, T1.DATE as morning_date,
                T2.trip_id as afternoon_trip_id, T2.duration_sec as afternoon_duration_sec, T2.start_date as afternoon_start_date, T2.start_station_name as afternoon_start_name,
                T2.start_station_id as afternoon_start_id, T2.end_date as afternoon_end_date, T2.end_station_name as afternoon_end_name, T2.end_station_id as afternoon_end_id,
                T2.bike_number as afternoon_bike_number, T2.zip_code as afternoon_zip_code, T2.subscriber_type as afternoon_subscriber_type, T2.HOUR as afternoon_hour, T2.DATE as afternoon_date,
            FROM `hanan-w205.Project_1_station_temp.Trips_with_hour` T1
            INNER JOIN `hanan-w205.Project_1_station_temp.Trips_with_hour` T2 ON 
                T1.DATE = T2.DATE
                AND T1.start_station_id = T2.end_station_id
                AND T1.end_station_id = T2.start_station_id
                AND T1.zip_code = T2.zip_code
                AND T1.subscriber_type = T2.subscriber_type
                AND ABS((T1.duration_sec - T2.duration_sec)/T1.duration_sec) < 0.2
            WHERE T1.duration_sec > 90 AND T1.duration_sec < 3600 AND T2.duration_sec > 90 AND T2.duration_sec < 3600
                AND T1.start_station_id <> T1.end_station_id
                AND T2.start_station_id <> T2.end_station_id
                AND T1.HOUR < 10 AND T1.HOUR > 4 AND T2.HOUR > 14 AND T2.HOUR < 21
          )
        )
    WHERE Morning_ID_Count = 1
    ORDER BY morning_trip_id
```

- Question 2: On which days do people use the service more? And on which days do they use it less?
  * Answer: The most popular days are Tuesday, Wednesday and Thursday, which are also typically when people go the office more. The least popular days are Saturday and Sunday, which also strenghens the point that most people use the service for commuting.
  * SQL query:

```sql
    SELECT weekday_name, ROUND(Count / SUM(Count) over(),2) percentage_of_week
    FROM (
        SELECT weekday_name, count(*) as Count
        FROM ( 
            SELECT FORMAT_DATE('%A', start_date) AS weekday_name
            FROM `bigquery-public-data.san_francisco.bikeshare_trips`
        )
        GROUP BY weekday_name
    )
    ORDER BY 
         CASE
              WHEN weekday_name = 'Monday' THEN 1
              WHEN weekday_name = 'Tuesday' THEN 2
              WHEN weekday_name = 'Wednesday' THEN 3
              WHEN weekday_name = 'Thursday' THEN 4
              WHEN weekday_name = 'Friday' THEN 5
              WHEN weekday_name = 'Saturday' THEN 6
              WHEN weekday_name = 'Sunday' THEN 7
         END ASC
```

- Question 3: How many of the commuters trips are done by users subscribed the the service, vs. non-subscribers? And how many of all uses?
  * Answer: From the general trips population, 86% are of subscribed users. For the trips recognized as commuter trips, 98.6% are of subscribed users.
  * SQL query:

```sql
    SELECT subscriber_type, count(*) as Count
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    GROUP BY subscriber_type;
    SELECT morning_subscriber_type, count(*) as Count
    FROM `hanan-w205.Project_1_station_temp.trip_joined`
    GROUP BY morning_subscriber_type;
```
  
- Question 4: Which stations are commuters typically using? Are many of them coming/going to the caltrain/ferry?
  * Answer: It seems like most users are indeed coming from or going to the stations near the ferry or caltrain.
  * SQL query:
  
```sql  
    SELECT morning_start_name, morning_end_name, count(*) as COUNT
    FROM `hanan-w205.Project_1_station_temp.trip_joined` T1
    INNER JOIN `bigquery-public-data.san_francisco.bikeshare_stations` T2 ON T1.morning_start_name = T2.name
    WHERE landmark = "San Francisco"
    GROUP BY morning_start_name, morning_end_name
    ORDER BY COUNT DESC
```
  

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

