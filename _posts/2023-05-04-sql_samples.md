---
layout: post
title:  "SQL Query Showcase"
date: "2023-05-04"
categories: misc
tags: [sql]
---

This is a showcase of sample SQL queries that I made for practice in learning the language. The sources listed before each query link to the direct place the content came from. Each heading represents a concept of SQL that I touched on. 

***Sources:***
[DataLemur](https://datalemur.com/) by Nick Singh 

[SQL-practice.com](https://www.sql-practice.com/) by Boolean-algebra

## Common Table Expression (CTE)

*Source: (<https://datalemur.com/questions/sql-spare-server-capacity>)*

```sql
WITH demands AS (
  SELECT 
    datacenter_id,
    SUM(monthly_demand) AS total_demand
  FROM forecasted_demand
  GROUP BY datacenter_id)


SELECT demands.datacenter_id, demands.total_demand, 
        centers.name, centers.monthly_capacity AS total_capacity 
FROM demands 
  INNER JOIN datacenters AS centers ON demands.datacenter_id = centers.datacenter_id;
```

------------------------------------------------------------------------

## Subquery example

*Source: (<https://datalemur.com/questions/sql-spare-server-capacity>)*

```sql
SELECT centers.datacenter_id, (centers.monthly_capacity -
  demands.total_demand) AS spare_capacity 
FROM ( SELECT datacenter_id,
  SUM(monthly_demand) AS total_demand FROM forecasted_demand GROUP BY
  datacenter_id) AS demands 
INNER JOIN datacenters AS centers ON
  demands.datacenter_id = centers.datacenter_id 
ORDER BY centers.datacenter_id;
```

------------------------------------------------------------------------

## Aggregate Functions with SUM

*Source: (<https://www.sql-practice.com/>)* 

**Keyword: SUM, Difficulty: HARD**

```sql
SELECT CASE WHEN (patient_id % 2 = 0) THEN 'Yes' 
            ELSE 'No' 
        END AS has_insurance, 
        SUM( 
          CASE WHEN patient_id % 2 = 0 THEN 10 
          ELSE 50 
          END) AS cost_after_insurance 
FROM admissions 
GROUP BY has_insurance
```

### *Show the provinces that has more patients identified as 'M' than 'F'...* 

**Keyword: HAVING, Difficulty: HARD**

```sql

--Query A

SELECT pr.province_name 
FROM patients AS pa 
JOIN province_names AS pr ON pa.province_id = pr.province_id 
GROUP BY pr.province_name 
HAVING SUM(gender = 'M') \> SUM(gender = 'F');


--Query B, using COUNT

SELECT pr.province_name 
FROM patients AS pa 
JOIN province_names AS pr ON pa.province_id = pr.province_id 
GROUP BY pr.province_name 
HAVING COUNT(CASE WHEN gender = 'M' THEN 1 END) \> COUNT( CASE WHEN gender = 'F' THEN 1 END);


--Query C, using a subquery and SUM

SELECT province_name 
FROM ( SELECT province_name, SUM(gender = 'M') AS n_male, SUM(gender = 'F') AS n_female 
FROM patients pa 
JOIN province_names pr ON pa.province_id = pr.province_id 
GROUP BY province_name ) 
WHERE n_male \> n_female
```


## Aggregate and then display as a percentage

**Keyword: COUNT, Difficulty: HARD**

```sql
SELECT concat( ROUND( ( SELECT COUNT(*) FROM patients WHERE gender = 'M'
) / CAST(COUNT(*) as FLOAT), 4 ) \* 100, '%' ) AS percent_of_male_patients 
FROM patients 
WHERE gender = 'M' 
GROUP BY gender

--or...

--Query B

SELECT Concat(round(100 \* avg(gender = 'M'), 2),'%') AS percent_of_male_patients 
FROM patients;

--Query C

SELECT round(100 \* avg(gender = 'M'), 2) \|\| '%' AS percent_of_male_patients 
FROM patients;
```

------------------------------------------------------------------------

## Window function example RANK()

*Source: (<https://datalemur.com/questions/sql-third-transaction>)*

```sql
SELECT user_id, spend, transaction_date 
FROM (SELECT user_id, spend, transaction_date, RANK() OVER (PARTITION BY user_id ORDER BY
transaction_date) AS rank_num FROM transactions) AS trans_num 
WHERE rank_num = '3' 
GROUP BY user_id, spend, transaction_date
```
---

## Sending vs. Opening Snaps (CTE, CASE WHEN, Aggregates)

*Source: (<https://datalemur.com/questions/time-spent-snaps>)*

```sql
WITH activity AS ( SELECT ab.age_bucket, 
              SUM(CASE WHEN act.activity_type = 'open' THEN act.time_spent ELSE 0 END) AS time_open, 
              SUM(CASE WHEN act.activity_type = 'send' THEN act.time_spent ELSE 0 END) AS time_sent,
              SUM(act.time_spent) as total_time FROM activities act JOIN age_breakdown ab ON act.user_id = ab.user_id 
WHERE act.activity_type IN ('open', 'send') 
GROUP BY ab.age_bucket)

SELECT age_bucket, ROUND(((time_sent / total_time)\* 100.0),2) AS sent_perc, 
                    ROUND(((time_open / total_time)\* 100.0),2) AS open_perc 
FROM activity
ORDER BY age_bucket DESC
```
---

## Window functions for calculating rolling averages based on tweets

*Source: (<https://datalemur.com/questions/rolling-average-tweets>)*

```sql
SELECT user_id, tweet_date, TRUNC(AVG(tweet_count) 
  OVER(PARTITION BY user_id ORDER BY tweet_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW), 2) AS rolling_avg_3days 
FROM (SELECT user_id, tweet_date, count(tweet_date) as tweet_count FROM tweets GROUP BY user_id, tweet_date) AS daily_tweets 
GROUP BY user_id, tweet_date, tweet_count
ORDER BY user_id ASC, tweet_date ASC;
```
---

## Compensation Outliers 

*Source: (<https://datalemur.com/questions/compensation-outliers>)*

```sql
WITH payout 
AS 
  (SELECT
	employee_id, 
	salary, 
	title,
	(AVG(salary) OVER (PARTITION BY title)) * 2 AS doub_avg,
	(AVG(salary) OVER (PARTITION BY title)) / 2 AS half_avg
FROM employee_pay)

SELECT 
    employee_id,
    salary,
    CASE  
      WHEN salary > doub_avg THEN 'Overpaid'
      WHEN salary < half_avg THEN 'Underpaid'
    END AS status
FROM payout
GROUP BY
  employee_id, salary, doub_avg, half_avg
HAVING
  salary > doub_avg OR
  salary < half_avg
ORDER BY 
  employee_id;
```

---

## Photoshop Revenue Analysis

*Source: (<https://datalemur.com/questions/photoshop-revenue-analysis>)*

```sql
SELECT customer_id, 
  SUM(revenue) as revenue
FROM adobe_transactions
WHERE customer_id IN (SELECT customer_id FROM adobe_transactions WHERE product = 'Photoshop') 
  AND product <> 'Photoshop'
GROUP BY customer_id
ORDER BY customer_id ASC
```

---

## Unique Money Transfer Relationships

*Source: (<https://datalemur.com/questions/money-transfer-relationships>)*

```sql
SELECT
  COUNT(payer_id) / 2
FROM (SELECT
        payer_id, recipient_id
      FROM payments
INTERSECT
      SELECT
        recipient_id, payer_id
      FROM payments) 
      as relationships;
```