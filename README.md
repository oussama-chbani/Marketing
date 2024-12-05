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

### **1. What are the total costs, earnings, and overall ROAS?**

To evaluate overall performance, the following queries were used:

#### **Total Costs and Earnings**
```sql
SELECT 
    ROUND(SUM(costs), 2) AS total_cost,
    ROUND(SUM(earnings), 2) AS total_earnings
FROM 
    marketing_data2;
