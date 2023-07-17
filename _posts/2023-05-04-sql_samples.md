---
layout: post
title:  "SQL Query Showcase"
date: "2023-05-04"
categories: misc
tags: [sql]
---

# Introduction: 

This is a showcase of sample SQL queries that I made for practice in learning the language. The sources listed before each query link to the direct place the content came from, unless noted otherwise. Each heading represents a concept of SQL that I touched on. 

**Primary Sources:**
[DataLemur](https://datalemur.com/) by Nick Singh 
[SQL-practice.com](https://www.sql-practice.com/) by Boolean-algebra

------------------------------------------------------------------------


## Common Table Expression (CTE)

*Source: Previous Interview Assessment* 

*Context: A restaurant company wants to use this dataset to inform management of trends with how customer order their products*


**Table: Customers**
| Index | customerOloRef | firstName | lastName | emailAddress    | phoneNumber |
|-------|----------------|-----------|----------|-----------------|-------------|
| 0     | 1210852832     | Ashlee    | Black    | 718798@test.com | 18539137645 |
| 1     | 1210862959     | Karen     | Taylor   | 908278@test.com | 17118714180 |
| 2     | 1211222409     | Rachel    | Glover   | 990342@test.com | 14160108199 |
| 3     | 1210884227     | Kevin     | Barnett  | 816118@test.com | 16375662719 |
| 4     | 1211205145     | Andrew    | Oneal    | 694351@test.com | 18535103663 |
| 5     | 1210892053     | Matthew   | Cobb     | 403207@test.com | 12141628213 |
| 6     | 1210878061     | Julie     | Rice     | 107345@test.com | 11470213737 |
| 7     | 1210868677     | Joshua    | Williams | 922872@test.com | 10627821511 |
| 8     | 1210850046     | Kimberly  | Miller   | 734453@test.com | 14769450538 |

...

**Table: Orders**
| Index | orderOloRef | customerOloRef | storeRef | timePlaced | orderedFromFave | source     | handoff        | paymentType | zipcode |
|-------|-------------|----------------|----------|------------|-----------------|------------|----------------|-------------|---------|
| 0     | 1526136030  | 1212424032     | 25       | 2021-08-10 | FALSE           | CallCenter | CurbsidePickup | CreditCard  | 60134   |
| 1     | 1636426442  | 1194539172     | 44       | 2021-08-10 | FALSE           | CallCenter | CurbsidePickup | CreditCard  | 65708   |
| 2     | 2116129966  | 1193104823     | 22       | 2021-08-10 | FALSE           | API        | CounterPickup  | CreditCard  | 60134   |
| 3     | 2112894463  | 1192380993     | 24       | 2021-08-10 | FALSE           | API        | Dispatch       | CreditCard  | 60543   |
| 4     | 1513983208  | 1194885843     | 13       | 2021-08-10 | FALSE           | API        | Dispatch       | CreditCard  | 60018   |
| 5     | 1645861726  | 1192678698     | 9        | 2021-08-10 | FALSE           | CallCenter | Dispatch       | CreditCard  | 60004   |
| 6     | 1868589889  | 1220411684     | 27       | 2021-08-10 | FALSE           | API        | CurbsidePickup | CreditCard  | 60431   |
| 7     | 1874887693  | 411086145      | 15       | 2021-08-10 | FALSE           | API        | Dispatch       | CreditCard  | 60107   |
| 8     | 1917609011  | 1191607575     | 37       | 2021-08-10 | FALSE           | CallCenter | Dispatch       | CreditCard  | 60452   |

...

**Table: Products**
| Index | orderOloRef | orderProduct            | quantity | cost  | orderedFromFave | source     | handoff        | paymentType | zipcode |
|-------|-------------|-------------------------|----------|-------|-----------------|------------|----------------|-------------|---------|
| 0     | 1332720190  | Chicken Tenders 6 Piece | 1        | 8.74  | FALSE           | CallCenter | CurbsidePickup | CreditCard  | 60134   |
| 1     | 1377148167  | Big Beef Sandwich       | 1        | 10.86 | FALSE           | CallCenter | CurbsidePickup | CreditCard  | 65708   |
| 2     | 1674285799  | Cheeseburger            | 1        | 0     | FALSE           | API        | CounterPickup  | CreditCard  | 60134   |
| 3     | 1560588602  | Caesar Salad            | 1        | 0     | FALSE           | API        | Dispatch       | CreditCard  | 60543   |
| 4     | 1514784642  | Large French Fries      | 3        | 3.74  | FALSE           | API        | Dispatch       | CreditCard  | 60018   |
| 5     | 1731086029  | Cheese Sauce            | 2        | 1.24  | FALSE           | CallCenter | Dispatch       | CreditCard  | 60004   |
| 6     | 1927656374  | Chocolate Cake Slice    | 2        | 4.36  | FALSE           | API        | CurbsidePickup | CreditCard  | 60431   |
| 7     | 1621969249  | *New Classic Beef Bowl  | 1        | 8.49  | FALSE           | API        | Dispatch       | CreditCard  | 60107   |
| 8     | 1781607712  | Italian Beef Sandwich   | 1        | 8.11  | FALSE           | CallCenter | Dispatch       | CreditCard  | 60452   |

...

**Step 1: Tell which customers order French Fries in their past orders.**

```sql 
SELECT *, 
    CASE WHEN orderproduct LIKE '%French Fries%' 
      THEN 'TRUE' ELSE 'FALSE' END AS isFrenchFry
FROM "Products"
GROUP BY index, orderproduct, orderoloref, quantity, cost;
```
**Step 2: What is the purchasing frequency of customers that order certain products?**

```sql 
SELECT DISTINCT p1.orderproduct as "product_a", 
                p2.orderproduct as "product_b", 
  COUNT(*) as PurchaseFrequency,
  COUNT(*) * 100.0/ SUM(COUNT(*)) OVER () as PercentFreq
FROM "Products" AS p1
INNER JOIN "Products" AS p2
ON p1.orderproduct <> p2.orderproduct
  AND p1.orderoloref = p2.orderoloref
GROUP BY p1.orderproduct, p2.orderproduct
ORDER BY PurchaseFrequency DESC;
```
**Step 3 & 4: What customers ordered the biggest quanitity of the most popular items on the menu?**

```sql
WITH porders AS 
  (
    SELECT CONCAT(cust.firstname, ' ', cust.lastname) AS name, 
    cust.emailaddress,
    cust.phonenumber,
    p1.orderproduct as "product_a", 
    p2.orderproduct as "product_b",
    SUM(p1.quantity) as "total_quantity",
    ROUND(SUM(p1.cost::numeric),2) as "total_cost",
    COUNT(*) as PurchaseFrequency,
    ROUND(COUNT(*) * 100.0/ SUM(COUNT(*)) OVER (), 3) as PercentFreq
FROM "Products" AS p1
INNER JOIN "Products" AS p2
ON p1.orderproduct <> p2.orderproduct
    AND p1.orderoloref = p2.orderoloref
JOIN "Orders" AS ord 
  ON ord.orderoloref = p1.orderoloref
JOIN "Customers" AS cust 
  ON cust.customeroloref = ord.customeroloref
WHERE p1.orderproduct 
  LIKE '%French Fries%'
GROUP BY p1.orderproduct, p2.orderproduct, p1.cost, cust.firstname, cust.lastname, cust.emailaddress, cust.phonenumber
ORDER BY PurchaseFrequency DESC
) 
SELECT * 
FROM porders;
```
...

*Source: Previous Interview Assessment*

*Context: What are the total revenue across months for the organization and what are noticable changes that I can observe from the data overtime for the last calendar year?*
 
```sql
WITH CTE AS(
SELECT DATE_TRUNC('month', CURRENT_MONTH) AS month, 
        YEAR(DATE_TRUNC('month', CURRENT_MONTH)) AS year,
        ROUND((SUM(unit_value)) OVER (PARTITION BY DATE_TRUNC('month', CURRENT_MONTH)), 2) AS total_monthly_unit_value,
        ROUND((SUM(mrr_value)) OVER (PARTITION BY DATE_TRUNC('month', CURRENT_MONTH)), 2) AS total_mrr_value,
        ROUND((SUM(transaction_value)) OVER (PARTITION BY DATE_TRUNC('month', CURRENT_MONTH)), 2) AS total_monthly_transaction_value
FROM DATA_ANALYST_INTERVIEW.PUBLIC.HISTORICAL_MRR_TRANSACTIONS
    WHERE CURRENT_MONTH BETWEEN '2021-01-01' AND '2022-12-01'
GROUP BY DATE_TRUNC('month', CURRENT_MONTH)
ORDER BY DATE_TRUNC('month', CURRENT_MONTH)
)

SELECT MONTHNAME(month) AS DATE, year, total_monthly_unit_value, 
    ROUND(((total_monthly_unit_value/lag(total_monthly_unit_value, 1) OVER (ORDER BY DATE_TRUNC('month', month))) - 1) * 100, 1) AS UV_percentage_change,
      total_mrr_value, 
    ROUND(((total_mrr_value/lag(total_mrr_value, 1) OVER (ORDER BY DATE_TRUNC('month', month))) - 1) * 100, 1) AS MRR_percentage_change,
      total_monthly_transaction_value, 
    ROUND(((total_monthly_transaction_value/lag(total_monthly_transaction_value, 1) OVER (ORDER BY DATE_TRUNC('month', month))) - 1) * 100, 1) AS TV_percentage_change,
    ROUND((UV_PERCENTAGE_CHANGE + MRR_PERCENTAGE_CHANGE + TV_PERCENTAGE_CHANGE)/3, 1) as AVERAGE_PERCENTAGE_CHANGE
FROM CTE
GROUP BY month, year, total_monthly_unit_value, total_mrr_value, total_monthly_transaction_value;

-- What is the total revenue by account type? 
SELECT  pda.account_type, ROUND(SUM(hmrr.transaction_value), 2) AS Revenue
FROM DATA_ANALYST_INTERVIEW.PUBLIC.HISTORICAL_MRR_TRANSACTIONS AS hmrr
    JOIN DATA_ANALYST_INTERVIEW.PUBLIC.DIM_ACCOUNTS AS pda ON pda.owner_user_id = hmrr.owner_id
GROUP BY account_type
ORDER BY pda.account_type;
```
...

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