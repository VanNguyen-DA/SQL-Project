# Ecommerce Web Performance & Purchase Behavior Analysis | SQL, BigQuery

<img width="950" height="570" alt="image" src="https://github.com/user-attachments/assets/fdad4a87-135e-4af8-9184-d8a53b14d64d" />

**Author:** Nguyen Thi Thanh Van  
**Tools:** SQL, Google BigQuery

---

## 📑 Table of Contents

- Background Overview
- Dataset Description & Data Structure
- Analysis process
- Final Conclusion & Recommendations

---

# 📌 Background & Overview

## What is this project about?

This project analyzes the **Google Analytics Sample E-commerce Dataset** using **SQL in Google BigQuery** to evaluate website performance, customer engagement, purchasing behavior, and revenue generation. The analysis transforms raw session-level data into actionable business insights that support marketing optimization and revenue growth.

## ❓ Business Questions

This project aims to answer the following key business questions:

✔️ How did website traffic perform in terms of **visits, pageviews, and transactions**?

✔️ Which traffic sources generated the highest **bounce rates** and conversion performance?

✔️ Which marketing channels contributed the most **revenue**?

✔️ How does user engagement differ between **purchasers and non-purchasers**?

✔️ How frequently do customers make repeat purchases?

✔️ Which devices contribute the most revenue?

✔️ What products are commonly purchased together?

✔️ How effective is the customer journey from **product view → add to cart → purchase**?

✔️ How does revenue accumulate over time?

These insights help businesses improve acquisition strategies, optimize conversion funnels, and increase customer value.

## 👤 Who is this project for?

✔️ Data Analysts & Business Analysts

✔️ E-commerce Managers

✔️ Digital Marketing Teams

✔️ Product & Growth Teams

✔️ Business Intelligence Professionals

---

# 📂 Dataset Description & Data Structure

### Data Source

The dataset comes from the **Google Analytics Sample Dataset** publicly available in **Google BigQuery**. It contains session-level data from the **Google Merchandise Store**, including website traffic, user interactions, transactions, products, and revenue information.

### Dataset

```ga4_obfuscated_sample_ecommerce```

### How to access data

### 📌 How to Access the Dataset

1. Log in to your **Google Cloud Platform** account and create a new project.
2. Open the **BigQuery Console** and select your project.
3. Click **Add Data** in the navigation panel, then choose **Search a project**.
4. Enter the following dataset in the search bar:

```bigquery-public-data.google_analytics_sample.ga_sessions```

5. Open the dataset and explore the tables:

```ga_sessions_```

6. Start querying the data using BigQuery SQL.

### 📌 Key Fields

| Field | Description |
|---------|-------------|
| fullVisitorId | Unique user identifier |
| date | Session date |
| trafficSource.source | Acquisition source |
| totals.visits | Number of visits |
| totals.pageviews | Number of pageviews |
| totals.transactions | Number of transactions |
| totals.bounces | Bounce sessions |
| device.deviceCategory | Desktop, Mobile, Tablet |
| hits.product.v2ProductName | Product name |
| hits.product.productRevenue | Product revenue |
| hits.product.productQuantity | Purchased quantity |

The dataset contains nested structures and arrays, requiring the use of **UNNEST()** to analyze product-level and transaction-level data.

### 📌 Skills Demonstrated

- SQL Querying
- Data Aggregation
- Common Table Expressions (CTEs)
- Window Functions
- Cohort Analysis
- Funnel Analysis
- Customer Behavior Analysis
- Revenue Analysis
- BigQuery UNNEST Operations

---

# ⚒️ Analysis Process

The analysis consists of 10 SQL business scenarios:

## 🔍 1. Calculate Total Visits, Pageviews, and Transactions for January, February, and March 2017

### 📖 Requirement Explanation

The objective of this analysis is to evaluate the overall website performance during the first quarter of 2017 by measuring three key metrics:

- **Visits**: Total number of website sessions.
- **Pageviews**: Total number of pages viewed by visitors.
- **Transactions**: Total number of completed purchases.

By comparing these metrics across January, February, and March 2017, we can identify trends in website traffic, user engagement, and sales performance. This provides a high-level overview of business growth and helps assess whether website performance improved over the three-month period.

### 🚀 Query

```
SELECT
    FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month,
    SUM(totals.visits) AS visits,
    SUM(totals.pageviews) AS pageviews,
    SUM(totals.transactions) AS transactions
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
WHERE FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date))
      IN ('201701','201702','201703')
GROUP BY month
ORDER BY month ASC;
```

### 💡 Query Result

<img width="1105" height="301" alt="image" src="https://github.com/user-attachments/assets/3ee9d7b6-df16-47b5-9bb2-5cdbc1283cbd" />


## 🔍 2. Bounce Rate by Traffic Source in July 2017
 
### 📖 Requirement Explanation
 
The objective of this analysis is to evaluate the quality of website traffic by measuring the **bounce rate** for each traffic source in July 2017.
 
A bounce occurs when a visitor lands on the website and leaves without interacting with any other page. Therefore, bounce rate is a key indicator of user engagement and landing page effectiveness.
 
**Formula:**
 
```text
Bounce Rate = Number of Bounces / Total Visits × 100
```
 
This analysis helps answer the following questions:
 
- Which traffic sources bring the highest volume of visitors?
- Which channels drive more engaged users?
- Which sources may require optimization due to high bounce rates?
 
The results can help marketers improve campaign targeting, optimize landing pages, and increase traffic quality.
 
### 🚀 Query
 
```
SELECT
trafficSource.source AS source,
SUM(totals.visits) AS total_visits,
SUM(totals.bounces) AS total_no_of_bounces,
ROUND(
SAFE_DIVIDE(
SUM(totals.bounces),
SUM(totals.visits)
) * 100,
2
) AS bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
GROUP BY source
ORDER BY total_visits DESC;
```
 
### 💡 Query Result
 
<img width="1106" height="771" alt="image" src="https://github.com/user-attachments/assets/3a347fea-dfb8-436d-adb9-5c5b0a90b14b" />


## 🔍 3. Revenue by Traffic Source by Week and Month in June 2017

### 📖 Requirement Explanation

Analyze revenue generated by each traffic source in June 2017 on both a weekly and monthly basis to identify the most profitable acquisition channels.

### 🚀 Query

```select 
  'month' as time_type,
  format_date('%Y%m', parse_date('%Y%m%d', date)) as time,
  trafficSource.source AS source,
  round(sum(product.productRevenue) / 1000000, 4) as revenue
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
 unnest (hits) as hit, 
 unnest (hit.product) as product
 
 where product.productRevenue is not null
 group by time_type, time, source
 
 Union all 
 
 select 
  'week' as time_type,
  format_date('%Y%m', parse_date('%Y%m%d', date)) as time,
  trafficSource.source AS source,
  round(sum(product.productRevenue) / 1000000, 4) as revenue
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
 unnest (hits) as hit, 
 unnest (hit.product) as product
 
 where product.productRevenue is not null
 group by time_type, time, source;
```
### 💡 Query Result

<img width="1177" height="778" alt="image" src="https://github.com/user-attachments/assets/2297ef96-83b6-4c11-8366-6cf082282651" />

---

## 🔍 4. Conversion Rate by Traffic Source in 2017

### 📖 Requirement Explanation

Measure the effectiveness of each traffic source in converting visitors into customers.

**Formula:**

```text
Conversion Rate = Transactions / Visits
```

### 🚀 Query
```select 
 trafficSource.source as source,
 sum (totals.visits) as visits,
 sum (totals.transactions) as transactions,
 round (safe_divide (sum (totals.transactions),sum (totals.visits)),2) as conversion_rate
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
 
 group by source
 
 order by conversion_rate desc
 limit 50;
```

### 💡 Query Result

<img width="1093" height="773" alt="image" src="https://github.com/user-attachments/assets/c7294697-54a5-4432-b6f9-13680ee65527" />


---

## 🔍 5. Average Pageviews by Purchaser Type

### 📖 Requirement Explanation

Compare browsing behavior between purchasers and non-purchasers during June and July 2017.
### 🚀 Query

``` select
  format_date('%Y%m', parse_date('%Y%m%d', date)) as month,
  round(
  sum(case when totals.transactions >= 1 and product.productRevenue is not null 
  then totals.pageviews else 0 end)
  / Count (distinct case when totals.transactions >= 1 and product.productRevenue is not null
  then fullVisitorId end), 8
  ) as avg_pageviews_purchase,
  round(
  sum(case when totals.transactions is null and product.productRevenue is null 
  then totals.pageviews else 0 end)
  / count(distinct case when totals.transactions is null and product.productRevenue is null 
  then fullVisitorId end), 8
  ) as avg_pageviews_non_purchase
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
 unnest (hits) as hit, 
 unnest (hit.product) as product
 
 where 
 format_date('%Y%m', parse_date('%Y%m%d', date)) in ('201706','201707')
 
 group by month
 order by month;
 ```

### 💡 Query Result

<img width="1100" height="266" alt="image" src="https://github.com/user-attachments/assets/d2f061c5-1eba-48ae-a066-9722afb44915" />

---

## 🔍 6. Average Transactions per Purchasing User in July 2017

### 📖 Requirement Explanation

Calculate the average number of transactions made by users who completed at least one purchase.

### 🚀 Query
``` 
with device_revenue as (
 
 select
  device.deviceCategory as device,
  round(sum(product.productRevenue) / 1000000, 4) as revenue_by_device
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_*`,
 UNNEST(hits) AS hit,
 UNNEST(hit.product) AS product
 
 where totals.transactions is not null
 and product.productRevenue is not null
 
 group by device),
 
 total as (
  select 
  round(sum(product.productRevenue) / 1000000, 4) as total_revenue
 
  from `bigquery-public-data.google_analytics_sample.ga_sessions_*`,
 UNNEST(hits) AS hit,
 UNNEST(hit.product) AS product
 
 where totals.transactions is not null
 and product.productRevenue is not null
 
 )
 
 select 
  d.device,
  d.revenue_by_device,
  t.total_revenue,
  round((d.revenue_by_device / t.total_revenue) * 100, 2) as ratio
 from device_revenue as d
 cross join total as t
 order by ratio desc;
 ```

### 💡 Query Result

<img width="1099" height="227" alt="image" src="https://github.com/user-attachments/assets/f89fa13a-c1df-482e-9b66-2f575c2d2537" />

---

## 🔍 7. Revenue Contribution by Device in 2017

### 📖 Requirement Explanation

Measure the revenue contribution of each device category, including Desktop, Mobile, and Tablet.

### 🚀 Query

``` with device_revenue as (
 
 select
  device.deviceCategory as device,
  round(sum(product.productRevenue) / 1000000, 4) as revenue_by_device
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_*`,
 UNNEST(hits) AS hit,
 UNNEST(hit.product) AS product
 
 where totals.transactions is not null
 and product.productRevenue is not null
 
 group by device),
 
 total as (
  select 
  round(sum(product.productRevenue) / 1000000, 4) as total_revenue
 
  from `bigquery-public-data.google_analytics_sample.ga_sessions_*`,
 UNNEST(hits) AS hit,
 UNNEST(hit.product) AS product
 
 where totals.transactions is not null
 and product.productRevenue is not null
 
 )
 
 select 
  d.device,
  d.revenue_by_device,
  t.total_revenue,
  round((d.revenue_by_device / t.total_revenue) * 100, 2) as ratio
 from device_revenue as d
 cross join total as t
 order by ratio desc;
 ```

### 💡 Query Result

<img width="1111" height="298" alt="image" src="https://github.com/user-attachments/assets/55a0ab61-d6f3-41ba-b6d3-6ed07cbf64af" />

---

## 🔍 8. Products Purchased Together with "YouTube Men's Vintage Henley"

### 📖 Requirement Explanation

Identify products frequently purchased by customers who also purchased **YouTube Men's Vintage Henley** to uncover cross-selling opportunities.

### 🚀 Query
```with target_users as (
  select distinct fullVisitorId
  from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
  UNNEST(hits) as hit,
  UNNEST(hit.product) as product
  where
  totals.transactions >= 1
  and product.productRevenue is not null
  and product.v2ProductName = "YouTube Men's Vintage Henley"
 )
 
 select
  product.v2ProductName as other_purchased_products,
  sum(product.productQuantity) as quantity
 from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
  UNNEST(hits) as hit,
  UNNEST(hit.product) as product
 where
  totals.transactions >= 1
  and product.productRevenue is not null
  and fullVisitorId in (select fullVisitorId from target_users)
  and product.v2ProductName != "YouTube Men's Vintage Henley"
 group by other_purchased_products
 order by quantity desc;
 ```

### 💡 Query Result

<img width="1096" height="778" alt="Screenshot 2026-10-04 150506" src="https://github.com/user-attachments/assets/1c5c2acd-c5da-4229-be00-9a779b82246e" />

---

## 🔍 9. Conversion Funnel Analysis (View → Cart → Purchase)

### 📖 Requirement Explanation

Track user progression through the purchase funnel from product view to add-to-cart and purchase during January-March 2017.

```text
Product View
    ↓
Add to Cart
    ↓
Purchase
```

### 🚀 Query
```with product_data as(
select
    format_date('%Y%m', parse_date('%Y%m%d',date)) as month,
    count(CASE WHEN eCommerceAction.action_type = '2' THEN product.v2ProductName END) as num_product_view,
    count(CASE WHEN eCommerceAction.action_type = '3' THEN product.v2ProductName END) as num_add_to_cart,
    count(CASE WHEN eCommerceAction.action_type = '6' and product.productRevenue is not null THEN product.v2ProductName END) as num_purchase
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_*`
,UNNEST(hits) as hits
,UNNEST (hits.product) as product
where _table_suffix between '20170101' and '20170331'
and eCommerceAction.action_type in ('2','3','6')
group by month
order by month
)

select
    *,
    round(num_add_to_cart/num_product_view * 100, 2) as add_to_cart_rate,
    round(num_purchase/num_product_view * 100, 2) as purchase_rate
from product_data;
```

### 💡 Query Result

<img width="1331" height="314" alt="image" src="https://github.com/user-attachments/assets/798170bb-1836-4d64-bcf5-b9ab55352d5e" />

---

## 🔍 10. Weekly and Cumulative Revenue (May - July 2017)

### 📖 Requirement Explanation

Analyze weekly revenue trends and calculate cumulative revenue to monitor overall business growth.

### 🚀 Query

```select
  week,
  weekly_revenue,
  round(sum(weekly_revenue) 
  over (order by week
  rows between unbounded preceding and current row), 2) as cumulative_revenue
 from (
  select
  format_date('%Y-%W', parse_date('%Y%m%d', date)) as week,
  round(sum(product.productRevenue) / 1000000, 2) as weekly_revenue
 
 from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
 unnest(hits) as hit,
 unnest(hit.product) as product
 
 where product.productRevenue is not null
 and parse_date('%Y%m%d', date) between date ('2017-05-01') and date ('2017-07-31')
 
 GROUP BY week)
 ORDER BY week;
```

### 💡 Query Result

<img width="1100" height="737" alt="image" src="https://github.com/user-attachments/assets/1dd0b7ed-cb6c-461b-aa67-2319e76c884a" />


# 🔎 Final Conclusion & Recommendations

The final section summarizes business insights and provides actionable recommendations derived from the analysis.

| Aspect | Insight | Recommendation |
|----------|----------|---------------|
| Traffic Performance | Understand website growth and engagement trends | Focus on channels driving quality traffic |
| Conversion Performance | Identify high-converting traffic sources | Optimize marketing budget allocation |
| Customer Behavior | Compare purchasers and non-purchasers | Improve engagement strategies for potential buyers |
| Revenue Contribution | Measure revenue by source and device | Prioritize top-performing channels and devices |
| Product Affinity | Discover products frequently purchased together | Implement cross-selling and bundle promotions |
| Purchase Funnel | Identify drop-off points in customer journey | Optimize product pages and checkout process |
| Revenue Growth | Track revenue trends over time | Support forecasting and business planning |
