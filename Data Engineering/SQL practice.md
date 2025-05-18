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

##### Average Post Hiatus

https://datalemur.com/questions/sql-average-post-hiatus-1

```sql
WITH post_twentyone AS (
  SELECT user_id
  FROM posts
  WHERE EXTRACT(YEAR FROM post_date) = '2021'
  GROUP BY user_id
  HAVING COUNT(*) > 1
)

SELECT p.user_id,
      EXTRACT (DAYS FROM MAX(p.post_date) - MIN(p.post_date)) AS days_between
FROM posts AS p 
INNER JOIN post_twentyone as pt 
ON p.user_id = pt.user_id 
GROUP BY p.user_id
```

CTE was unnecessary but was a good problem to practice. Needed to look up how to grab the day or year from a timestamp.

##### Teams Power Users

https://datalemur.com/questions/teams-power-users

```sql
SELECT sender_id,
       COUNT(*) AS message_count
FROM messages 
WHERE EXTRACT(YEAR FROM sent_date) = '2022' AND EXTRACT(MONTH FROM sent_date) = '08'
GROUP BY sender_id
ORDER BY message_count DESC
LIMIT 2
```

##### Duplicate Job Listings

https://datalemur.com/questions/duplicate-job-listings

```sql 
WITH test AS (
  SELECT company_id, title, description FROM job_listings
  GROUP BY company_id, title, description
  HAVING COUNT(*) > 1
)

SELECT COUNT(*)
FROM test
```

##### Cities With Completed Orders

https://datalemur.com/questions/completed-trades

```sql
SELECT 
	city, 
	COUNT(*) AS total_orders
FROM trades AS t 
LEFT JOIN users AS u ON t.user_id = u.user_id 
WHERE 
	t.status = 'Completed'
GROUP BY city 
ORDER BY COUNT(*) DESC
LIMIT 3
```

### Average Review Ratings

https://datalemur.com/questions/sql-avg-review-ratings

```sql
SELECT 
  EXTRACT(MONTH FROM submit_date) AS mth,
  product_id AS product,
  ROUND(AVG(stars),2) AS avg_stars

FROM reviews
GROUP BY EXTRACT(MONTH FROM submit_date), product_id
ORDER BY mth
```

##### Well Paid Employees

https://datalemur.com/questions/sql-well-paid-employees

```sql
SELECT e.employee_id,
       e.name
FROM employee AS e 
LEFT JOIN employee AS m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary
```


##### Second Day Confirmation 

https://datalemur.com/questions/second-day-confirmation

```sql
SELECT e.user_id  
FROM emails AS e 
LEFT JOIN texts AS T ON e.email_id = t.email_id 
WHERE t.signup_action = 'Confirmed' AND EXTRACT (DAYS FROM t.action_date - e.signup_date) = 1
```

