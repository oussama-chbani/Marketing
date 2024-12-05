# 📊 Marketing Data Case Study: Analyzing ROAS and Performance Metrics

## 📋 Objective

The goal of this case study is to analyze marketing data to evaluate the performance of different marketing channels and traffic sources. The primary focus is on calculating **Return on Advertising Spend (ROAS)** and deriving insights to optimize advertising budgets and maximize revenue.

---

## 🔍 Questions Addressed in the Case Study

### 1. **What are the total costs, earnings, and ROAS across all campaigns?**
   To understand the overall performance of marketing efforts.

### 2. **Which marketing channels and traffic sources deliver the highest ROAS?**
   Identifying the most effective channels to optimize budget allocation.

### 3. **How does performance vary over time (weekly/monthly)?**
   Understanding trends and seasonality for better planning and forecasting.

### 4. **Are there any anomalies in the dataset?**
   Detecting data inconsistencies or errors, such as future-dated entries.

### 5. **What combinations of marketing channels and traffic sources drive the best results?**
   Gaining granular insights into campaign performance by specific source-channel combinations.

---

## 🛠️ How Each Question Was Answered


### 1. Total Costs, Earnings, and Overall ROAS**
```sql
SELECT 
    ROUND(SUM(costs), 2) AS total_cost,
    ROUND(SUM(earnings), 2) AS total_earnings,
    ROUND(
        CASE 
            WHEN SUM(costs) = 0 THEN 0
            ELSE (SUM(earnings) / SUM(costs)) * 100
        END, 
        2
    ) AS total_ROAS_percentage
FROM 
    marketing_data2;

---

### 2. ROAS by Marketing Channel**
```sql
SELECT 
    mkt_channel,
    SUM(earnings) AS total_earnings,
    SUM(costs) AS total_costs,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    mkt_channel
ORDER BY 
    ROAS DESC;

### 3. ROAS by Traffic Source**
```sql
SELECT 
    traffic_source,
    SUM(earnings) AS total_earnings,
    SUM(costs) AS total_costs,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    traffic_source
ORDER BY 
    ROAS DESC;

### 4. ROAS by Marketing Channel and Traffic Source**
```sql
SELECT 
    mkt_channel,
    traffic_source,
    SUM(earnings) AS total_earnings,
    SUM(costs) AS total_costs,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    mkt_channel, 
    traffic_source
ORDER BY 
    ROAS DESC;

### 5. Monthly Performance Trends**
```sql
SELECT 
    DATE_TRUNC('month', partition_date) AS month,
    ROUND(SUM(costs), 2) AS total_costs,
    ROUND(SUM(earnings), 2) AS total_earnings,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    month
ORDER BY 
    month;

### 6. Weekly Performance Trends**
```sql
SELECT 
    DATE_TRUNC('week', partition_date) AS week_start,
    SUM(costs) AS total_costs,
    SUM(earnings) AS total_earnings,
    ROUND((SUM(earnings) / NULLIF(SUM(costs), 0)) * 100, 2) AS roas_percentage
FROM 
    marketing_data2
GROUP BY 
    week_start
ORDER BY 
    week_start;

### 7. Anomalous Data: Future-Dated Records**
```sql
SELECT *
FROM marketing_data2
WHERE partition_date > CURRENT_DATE;

### 8. (Optional) Delete Future-Dated Records**
```sql
DELETE FROM marketing_data2
WHERE partition_date > CURRENT_DATE;

### 9. Traffic Sources with Zero Costs**
```sql
SELECT 
    traffic_source,
    SUM(costs) AS total_costs,
    SUM(earnings) AS total_earnings,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    traffic_source
HAVING 
    SUM(costs) = 0
ORDER BY 
    traffic_source;

### 10. Monthly ROAS by Marketing Channel**
```sql
SELECT 
    DATE_TRUNC('month', partition_date) AS month,
    mkt_channel,
    SUM(earnings) AS total_earnings,
    SUM(costs) AS total_costs,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    month, mkt_channel
ORDER BY 
    month, mkt_channel;

### 11. Monthly ROAS by Traffic Source**
```sql
SELECT 
    DATE_TRUNC('month', partition_date) AS month,
    traffic_source,
    SUM(earnings) AS total_earnings,
    SUM(costs) AS total_costs,
    ROUND(SUM(earnings) / NULLIF(SUM(costs), 0), 2) AS ROAS
FROM 
    marketing_data2
GROUP BY 
    month, traffic_source
ORDER BY 
    month, traffic_source;

