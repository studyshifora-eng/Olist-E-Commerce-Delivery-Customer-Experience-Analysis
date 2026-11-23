# Olist-E-Commerce-Delivery-Customer-Experience-Analysis

## 1. Project Objective
Analyze e-commerce order fulfillment across Olist's multi-table dataset to identify **where delivery delays happen**, **how delays impact customer review scores**, and **which customer states and product categories drive operational friction**.

## 2. Business Questions
1. **Operational Baseline:** What are total delivered orders, total order value (item price + freight), overall late-delivery rate (%), and average delivery time (days)?
2. **Time Trends:** How do monthly order volumes and late-delivery rates evolve over time?
3. **Geographic & Category Bottlenecks:** Which customer states (`customer_state`) and product categories (translated from Portuguese to English) have the highest late-delivery rates and longest delay durations?
4. **Customer Satisfaction Impact:** How do average review scores (`review_score`) compare between on-time and late deliveries, and how do scores change across delay buckets (e.g., 1–3 days late, 4–7 days late, 8+ days late)?

## 3. Data Architecture & Scope
* **Source Dataset:** Brazilian E-Commerce Public Dataset by Olist (Kaggle).
* **Core Tables Used:**
  * `raw_orders` — order lifecycle timestamps and fulfillment status (`order_status`).
  * `raw_order_items` — line-item `price`, `freight_value`, and product/seller links.
  * `raw_products` & `raw_category_translation` — product attributes and English category mapping.
  * `raw_customers` — customer location (`customer_state`, `customer_city`).
  * `raw_order_reviews` — customer `review_score` and review timestamps.
* **Out of Scope:** Seller geolocation distance modeling and payment installment risk analysis.

## 4. Key Definitions & Data Quality Rules
* **Delivery Metrics Scope:** Calculated on completed orders (`order_status = 'delivered'`) with valid delivery timestamps so cancelled, unavailable, or unfulfilled orders do not distort delivery speed.
* **Late Delivery Flag:** Order delivered to customer after the estimated delivery date (`order_delivered_customer_date > order_estimated_delivery_date`).
* **Grain & Join Discipline:** Checked one-to-many relationships (such as multiple items per order or multiple reviews per order) prior to joining tables so row counts and revenue totals are not double-counted.

## 5. Tech Stack & Repository Structure
* **PostgreSQL (pgAdmin):** Raw data loading, data auditing, cleaning, and analytical SQL scripts.
* **Power BI Desktop:** Data modeling, DAX measures, and executive reporting.

```text
olist-delivery-analysis/
├── data_raw/        # Original Olist CSV files (unmodified)
├── data_clean/      # Cleaned tables/exports for analysis & BI
├── sql/             # Repeatable SQL scripts (audit, cleaning, analysis)
├── dashboard/       # Power BI report (.pbix) and summary visuals
└── README.md        # Project documentation
