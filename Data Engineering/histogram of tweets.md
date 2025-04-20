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

