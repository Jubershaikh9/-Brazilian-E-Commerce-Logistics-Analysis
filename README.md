# 🚚 Brazilian E-Commerce Logistics & Operations Analysis
**A Data-Driven Strategy for Supply Chain Optimization**

## 📌 Project Overview
As a **UK Master’s graduate in Logistics & Supply Chain** transitioning into **Data Science**, I conducted this deep-dive analysis on a dataset of **100,000+ orders** from Target Brazil (2016-2018). The goal was to identify operational bottlenecks, analyze revenue growth, and provide data-backed recommendations for fulfillment center expansion.

---

## 🚀 Key Business Insights
*   **136.98% Revenue Growth:** Analyzed Year-over-Year (YoY) performance (Jan-Aug 2017 vs 2018), identifying a massive scaling trend.
*   **Logistics Efficiency Gap:** Identified that while major hubs (SP, RJ, MG) have lower average freight costs, northern remote states face significantly higher costs and longer lead times.
*   **Seasonal Peak:** Discovered a massive spike in order volume every November, specifically driven by credit card transactions.
*   **Strategic Recommendation:** Proposed the development of **Regional Fulfillment Centers** in high-cost states (RR, AP, AM) to improve delivery speed and lower freight-to-value ratios.

---

## 🛠️ Tech Stack & Skills
*   **SQL (Google BigQuery):** CTEs, Joins, Window Functions (`LAG`, `DENSE_RANK`), and `TIMESTAMP_DIFF`.
*   **Data Cleaning:** Handling null values and ensuring data type consistency for 8+ tables.
*   **Domain Expertise:** Supply Chain Management (SCM), Inventory Planning, and KPI Tracking.

---

## 📊 Sample Queries & Logic

### 1. Calculating YoY Revenue Growth (Jan-Aug)
*Goal: To compare financial performance between 2017 and 2018.*

```sql
with order_cost as (
    select 
        extract(year from o.order_purchase_timestamp) as order_year,
        sum(p.payment_value) as cost_of_orders,
        lag(sum(p.payment_value)) over(order by extract(year from o.order_purchase_timestamp)) as previous_cost
    from `Target.orders` o
    inner join `Target.payments` p on o.order_id = p.order_id
    where extract(month from o.order_purchase_timestamp) between 1 and 8
    group by 1
)
select 
    order_year, 
    cost_of_orders,
    round(100 * (cost_of_orders - previous_cost) / previous_cost, 2) as growth_percentage
from order_cost;
```

### 2. Identifying Logistics Bottlenecks
*Goal: Ranking states by average delivery time to identify where micro-fulfillment centers are needed.*

```sql
select 
    c.customer_state,
    round(avg(timestamp_diff(o.order_delivered_customer_date, o.order_purchase_timestamp, day)), 2) as avg_delivery_days
from `Target.customers` c
inner join `Target.orders` o on c.customer_id = o.customer_id
where o.order_status = 'delivered'
group by 1
order by 2 desc
limit 5;
```

---

## 📂 Related Portfolios
*   **Full Case Study (PDF):** [View the Comprehensive Report Here](./Target_Brazil_Report.pdf)
*   **Retail Sales Dashboard (Tableau):** [Interactive Superstore Insights]([https://tableau.com](https://public.tableau.com/views/SuperstoreInteractiveDashboard_17744371159950/SuperstoreInteractiveInsightsDashboard?:language=en-GB&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link))
*   **HR Strategic Dashboard (Tableau):** [Workforce Management View]([https://tableau.com](https://public.tableau.com/views/Omega_HR_Dashboard_17732913735230/Dashboard1?:language=en-GB&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link))

---

## 📫 Contact & Connections
*   **LinkedIn:** [://linkedin.com](https://://linkedin.com)
*   **Current Focus:** Mastering Advanced Python & Machine Learning via Scaler.
