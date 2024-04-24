# SQL Project: YouTube Sport Channels Views using Subqueries

## Context
This mini SQL project is a series of questions asked by Jess Ramos around the topic of subqueries using a Sport Channels Views in Youtube dataset.

## Dataset
This dataset is available on [Kaggle](https://www.kaggle.com/datasets/kanchana1990/youtube-sports-channels-statistics?resource=download) and contains Youtube sport channels information.

### Definition

The file contains 558 rows with the following columns:
* **channel_id:** Unique identifier for each YouTube channel.
* **channel_title:** Name of the channel.
* **start_date:** The inception date of the channel on YouTube.
* **video_count:** Total number of videos uploaded by the channel.
* **view_count:** Cumulative number of views across all videos.
* **subscriber_count:** Total number of channel subscribers.

### Preview

<img src="https://github.com/mboss10/SQL-Project-YouTube-Sport-Channels-Views-using-Subqueries/blob/main/YoutubeSportChannelsPreview.png" width="900">

## The challenge
I am using my favorite SQL client [DBeaver](https://dbeaver.io). I have created a local SQLite database and imported each .csv file into a table. <br><br>
Below I am listing each question as they were given in the interview challenge, and underneath each I am providing a code block of the SQL queries I have written to solve the question, along with screenshot(s) of the results when relevant.

### Question 1
Using a subquery, identify the max number of rows for a single channel_id.
#### SQL code

```
-- The below selects the maximum rows column value amongst all from the subquery listing the number of rows per channel
SELECT 
	max(rows) as 'Max number of rows'
FROM (
-- The below subquery counts the number of rows per channel
	SELECT
		yscs.channel_id,
		count(1) as rows
	FROM 
		yt_sports_channels_stats yscs 
	GROUP BY
		yscs.channel_id
	)
```

#### Results
<img src="https://github.com/mboss10/SQL-Project-YouTube-Sport-Channels-Views-using-Subqueries/blob/main/Q1-results.png" width="400">

### Question 2
Using a subquery, identify which channel_ids have 2 rows.
#### SQL code

```
-- We select the channel id and count of rows for each	
SELECT 
	channel_id,
	rows 
FROM (	
-- The below subquery counts the number of rows per channel
	SELECT
		yscs.channel_id,
		count(1) as rows
	FROM 
		yt_sports_channels_stats yscs 
	GROUP BY
		yscs.channel_id
	)
-- then where clause filters on the channel with 2 rows
WHERE 
	rows = 2
```

#### Results
<img src="https://github.com/mboss10/SQL-Project-YouTube-Sport-Channels-Views-using-Subqueries/blob/main/Q2-results.png" width="400">

### Question 3
Rewrite your subqueries in #1 and #2 as CTEs to practice!
#### SQL code

```
/* Rewrite of 1 using CTE */	
 WITH cte_channel_rows AS (
 	SELECT
		yscs.channel_id,
		count(1) as rows
	FROM 
		yt_sports_channels_stats yscs 
	GROUP BY
		yscs.channel_id
 )
 SELECT 
 	max(rows) as 'Max number of rows'
 FROM 
 	cte_channel_rows
 	
 /* Rewrite of 2 using CTE */	
 WITH cte_channel_rows AS (
 	SELECT
		yscs.channel_id,
		count(1) as rows
	FROM 
		yt_sports_channels_stats yscs 
	GROUP BY
		yscs.channel_id
 )
 SELECT 
 	channel_id,
 	rows
 FROM 
 	cte_channel_rows
 WHERE 	
 	rows = 2
```

### Question 4
Bonus Question: pull the duplicate channel_ids using HAVING instead of subqueries or CTEs.
#### SQL code
```
 SELECT 
 	yscs.channel_id,
	count(1) as rows
FROM 
	yt_sports_channels_stats yscs
GROUP BY
	yscs.channel_id
HAVING COUNT(1) = 2
```
