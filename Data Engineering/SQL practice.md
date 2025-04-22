Resources:

https://datalemur.com/questions 

## Data Lemur

###### Histogram of Tweets

https://datalemur.com/questions/sql-histogram-tweets

```sql
WITH tweets_per_user AS (
  SELECT 
    COUNT(*) as num_tweets,
    user_id
  FROM tweets
  WHERE DATE(tweet_date) BETWEEN '2022-01-01' 
  AND '2022-12-31'
  GROUP BY user_id
)

SELECT 
  num_tweets AS tweet_bucket,
  COUNT(*) AS users_num
FROM tweets_per_user
GROUP BY num_tweets
```

##### Data Science Skills 

https://datalemur.com/questions/matching-skills

```sql 
SELECT candidate_id 
FROM candidates
WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(*) = 3
ORDER BY candidate_id ASC
```


###### Page With No Likes

https://datalemur.com/questions/sql-page-with-no-likes

```sql 
SELECT p.page_id 
FROM pages AS p 
LEFT JOIN page_likes AS pl ON p.page_id = pl.page_id
WHERE liked_date IS NULL
ORDER BY page_id
```

##### Unfinished Parts

https://datalemur.com/questions/tesla-unfinished-parts

```sql
SELECT part, assembly_step
FROM parts_assembly 
WHERE finish_date IS NULL
```


##### Laptop Vs Mobile Viewership

https://datalemur.com/questions/laptop-mobile-viewership

```sql
SELECT 
  COUNT(CASE WHEN device_type = 'laptop' THEN 1 END ) AS laptop_views,
  COUNT(CASE WHEN device_type IN ('tablet','phone') THEN 1 END) AS mobile_views
FROM viewership;
```