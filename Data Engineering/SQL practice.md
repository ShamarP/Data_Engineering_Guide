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

##### IBM db2 Product Analytics

```sql
WITH TOTALS AS (
  SELECT
    e.employee_id,
    SUM(
      CASE
        WHEN EXTRACT(YEAR FROM q.query_starttime + (q.execution_time * INTERVAL '1 second')) = 2023
         AND EXTRACT(MONTH FROM q.query_starttime + (q.execution_time * INTERVAL '1 second')) BETWEEN 7 AND 9
        THEN 1
        ELSE 0
      END
    ) AS counts
  FROM employees AS e
  LEFT JOIN queries  AS q
    ON e.employee_id = q.employee_id
  GROUP BY e.employee_id
)

SELECT 
  counts AS unique_queries,
  COUNT(employee_id) AS employee_count
FROM TOTALS
GROUP BY counts
ORDER BY unique_queries 

```

##### cards issued difference

https://datalemur.com/questions/cards-issued-difference

```sql
SELECT 
  card_name,
  MAX(issued_amount) - MIN(issued_amount) AS difference
FROM monthly_cards_issued
GROUP BY card_name
ORDER BY difference DESC
```

##### compressed mean

https://datalemur.com/questions/alibaba-compressed-mean

```sql
SELECT 
  ROUND(SUM(item_count::DECIMAL * order_occurrences) / SUM(order_occurrences),1) AS mean
FROM items_per_order
```

##### pharmacy analytics (part 1)

https://datalemur.com/questions/top-profitable-drugs

```sql 
SELECT 
  drug,
  total_sales - cogs AS total_profit
FROM pharmacy_sales
ORDER BY total_profit DESC
LIMIT 3
```

##### pharmacy analytics (part 2)

https://datalemur.com/questions/non-profitable-drugs

```sql
SELECT 
  manufacturer,
  COUNT(drug) AS drug_count,
  SUM(cogs - total_sales) AS total_loss
FROM pharmacy_sales
WHERE cogs > total_sales
GROUP BY manufacturer
ORDER BY total_loss DESC
```

##### Pharmacy Analytics (Part 3)

https://datalemur.com/questions/total-drugs-sales

```sql 
SELECT manufacturer,
       CONCAT('$', ROUND(SUM(total_sales) / 1000000), ' million')  AS sale
FROM pharmacy_sales
GROUP BY manufacturer
ORDER BY SUM(total_sales) DESC, manufacturer
```

##### Patient Support Analysis

https://datalemur.com/questions/frequent-callers

```sql
WITH TECH AS (
  SELECT 
    policy_holder_id,
    COUNT(case_id) AS cases
  FROM callers
  GROUP BY policy_holder_id
)

SELECT 
  COUNT(*) AS policy_holder_count
FROM TECH
WHERE cases >= 3
```


##### Users Third Transaction

https://datalemur.com/questions/sql-third-transaction

```sql
WITH rownums AS (
  SELECT user_id,
         spend,
         transaction_date,
         ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_date ) AS rn
  FROM transactions
)

SELECT user_id,
       spend,
       transaction_date
FROM rownums
WHERE rn = 3
```

##### Second Highest Salary

https://datalemur.com/questions/sql-second-highest-salary

```sql
WITH ranked_salaries AS (
  SELECT employee_id,
         salary,
         RANK() OVER (ORDER BY salary DESC) AS rn 
  FROM employee
)

SELECT salary
FROM ranked_salaries
WHERE rn = 2
LIMIT 1;
```

##### Sending Vs Opening Snaps

https://datalemur.com/questions/time-spent-snaps

```sql
WITH act AS (
SELECT 
  ab.age_bucket,
  SUM(CASE
          WHEN a.activity_type = 'open' THEN time_spent
          ELSE 0
      END
  ) AS open_time,
  SUM(CASE
          WHEN a.activity_type = 'send' THEN time_spent
          ELSE 0
      END
  ) AS send_time
FROM activities AS a 
LEFT JOIN age_breakdown AS ab ON a.user_id = ab.user_id
GROUP BY ab.age_bucket
)

SELECT 
    age_bucket,
    ROUND(send_time / (send_time + open_time ) * 100.0,2) AS send_perc,
    ROUND(open_time / (send_time + open_time ) * 100.0,2) AS open_perc
FROM act
```

##### Highest-Grossing Items

https://datalemur.com/questions/sql-highest-grossing

```sql
WITH product_spend2 AS (
  SELECT 
    category,
    product,
    SUM(spend) AS total_spend
  FROM product_spend
  WHERE EXTRACT(YEAR FROM transaction_date) = 2022
  GROUP BY category, product
),

product_ranking AS (
  SELECT 
    category,
    product,
    total_spend,
    RANK() OVER (
      PARTITION BY CATEGORY 
      ORDER BY total_spend DESC
    ) AS ranking
  FROM product_spend2
)

SELECT 
    category,
    product,
    total_spend
FROM product_ranking
WHERE ranking = 1 OR ranking = 2
```

Notes: Could have simplified this but I was over complicating it by trying to do a window function to calculate the total sum of a product within category... this led to duplicates which messed up my final solution but chat gpt helped me figure out why it caused duplicates

##### Top Three Salaries

https://datalemur.com/questions/sql-top-three-salaries

```sql
WITH ranked_salaries AS (
  SELECT 
      d.department_name,
      e.name,
      e.salary,
      DENSE_RANK () OVER (PARTITION BY 
      e.department_id ORDER BY e.salary DESC) AS rank
  FROM employee AS e 
  LEFT JOIN department AS d ON e.department_id = d.department_id

)

SELECT 
    department_name,
    r.name,
    salary
FROM ranked_salaries as r
WHERE rank <= 3
ORDER BY department_name, salary DESC, r.name 
```

Notes: Messed up on when to order the data but figured it out after reading the description a few times.