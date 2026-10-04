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

1. Traffic Performance Analysis
2. Bounce Rate Analysis
3. Revenue by Traffic Source
4. Conversion Rate Analysis
5. Purchaser vs Non-Purchaser Comparison
6. Average Transactions per Purchasing User
7. Revenue Contribution by Device
8. Product Affinity Analysis
9. Purchase Funnel (View → Cart → Purchase)
10. Weekly & Cumulative Revenue Analysis

For each scenario, the project includes:

- Business Objective
- SQL Query
- Query Output
- Key Findings

---

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
