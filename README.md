# Ecommerce Web Performance & Purchase Behavior Analysis | SQL, BigQuery

**Author:** Nguyen Thi Thanh Van  
**Tools:** SQL, Google BigQuery

---

# Table of Contents

- Background & Overview
- Dataset Description & Data Structure
- Analysis Process
- Final Conclusion & Recommendations

---

# Background & Overview

## What is this project about?

This project analyzes the Google Analytics E-commerce dataset stored in Google BigQuery to evaluate website performance, user engagement, purchase behavior, and revenue generation.

Using SQL, the analysis transforms raw web analytics data into meaningful business insights by examining key e-commerce metrics such as traffic volume, bounce rates, conversion rates, revenue contribution, customer purchasing patterns, and product performance. The project also investigates the customer journey from product view to purchase, helping identify opportunities to improve conversion and overall business growth. 【1-0c6f6d】

## Business Questions

The analysis addresses the following business questions:

1. How did website traffic perform in terms of visits, pageviews, and transactions?
2. Which traffic sources generated the highest bounce rates?
3. Which marketing channels contributed the most revenue?
4. Which traffic sources achieved the best conversion rates?
5. How does browsing behavior differ between purchasers and non-purchasers?
6. How frequently do customers make transactions after purchasing?
7. Which device categories contribute the most revenue?
8. Which products are commonly purchased together?
9. How effective is the conversion funnel from product view to add-to-cart and purchase?
10. How does revenue accumulate over time?

These insights support data-driven decisions related to marketing optimization, customer engagement, conversion improvement, and revenue growth. 【1-0c6f6d】

## Who is this project for?

- Data Analysts
- Business Analysts
- Marketing Analysts
- E-commerce Managers
- Data Science Students
- Anyone seeking hands-on experience with SQL and BigQuery

---

# Dataset Description & Data Structure

## Data Source

The dataset is based on the Google Analytics Sample E-commerce data available in BigQuery Public Datasets. It contains website session data from the Google Merchandise Store, including visitor activity, traffic acquisition channels, product interactions, transactions, and revenue information. 【1-0c6f6d】

## Dataset

```sql
bigquery-public-data.google_analytics_sample.ga_sessions_*
```

## Key Fields

| Field | Description |
|---------|-------------|
| fullVisitorId | Unique visitor identifier |
| date | Session date |
| trafficSource.source | Traffic acquisition source |
| totals.visits | Number of visits |
| totals.pageviews | Number of pageviews |
| totals.transactions | Number of transactions |
| totals.bounces | Bounce sessions |
| device.deviceCategory | Device category (Desktop, Mobile, Tablet) |
| hits.product.v2ProductName | Product name |
| hits.product.productRevenue | Product revenue |
| hits.product.productQuantity | Purchased quantity |
| hits.eCommerceAction.action_type | User interaction stage in purchase funnel |

The dataset contains nested and repeated fields, requiring the use of BigQuery's `UNNEST()` function to analyze product-level and event-level data. 【1-0c6f6d】

## How to Access the Dataset

1. Log in to Google Cloud Platform.
2. Create or select a BigQuery project.
3. Open BigQuery Console.
4. Search for:

```sql
bigquery-public-data.google_analytics_sample
```

5. Explore the `ga_sessions_*` tables and begin querying.

---

# Analysis Process

The project is organized into a series of SQL analyses, with each section including:

- Business Question
- SQL Query
- Query Result
- Key Findings

The analysis covers:

- Traffic Performance Analysis
- Bounce Rate Analysis
- Revenue Analysis
- Conversion Analysis
- Customer Behavior Analysis
- Device Performance Analysis
- Product Affinity Analysis
- Conversion Funnel Analysis
- Revenue Trend Analysis

Each query is designed to answer a specific business question and generate actionable insights from e-commerce data. 【1-0c6f6d】

---

# Final Conclusion & Recommendations

The final section summarizes findings from all analyses and translates them into business recommendations.

| Aspect | Insight | Recommendation |
|----------|----------|---------------|
| Traffic Performance | Identify

Ecommerce Web Performance & Purchase Behavior Analysis | SQL, BigQuery

Author: Nguyen Thi Thanh Van
Tools: SQL

Table of Contents
Background & Overview
Dataset Description & Data Structure
Final Conclusion & Recommendations

Background & Overview
What is this project about? What Business Question will it solve?

Who is this project for?

Dataset Description & Data Structure
DataSource: The sample data is from Google Analytics 4 (GA4), exported to BigQuery, including user activity data from the Google Merchandise Store e-commerce website.

Data Size: 
Dataset: ga4_obfuscated_sample_ecommerce

How to access this data:
Log in to your Google Cloud Platform account and create a new project.
Open the BigQuery Console and select your project.
Click on "Add Data" in the navigation panel, then choose "Search a project".
In the search bar, enter the project ID: bigquery-public-data.google_analytics_sample.ga_sessions and press Enter.
Click on the ga_sessions_ table to explore its structure and data.

Process => answer each question in excel file
explain each question and give querries and result after queries

Give Final Conclusion & Recommendation

Aspect | Insight | Recommendation
