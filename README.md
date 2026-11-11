# Customer Behavior Analytics Dashboard

End-to-end customer behavior analysis using **Python, PostgreSQL, and Power BI**.

![Python](https://img.shields.io/badge/Python-3.x-blue) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-SQL-336791) ![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811) ![License](https://img.shields.io/badge/License-MIT-green)

---

## Overview

This project analyzes the shopping behavior of **3,900 customers** to understand who buys, what they buy, and how much they spend. Data is cleaned and explored in Python, stored and queried in PostgreSQL, and visualized in an interactive Power BI dashboard.

**Business questions answered**
- How many customers do we have, and what are the average purchase amount and review rating?
- Which product categories and age groups drive the most revenue?
- How do subscribers compare with non-subscribers?
- Do gender, shipping type, season, or discounts influence spending?

---

## Dataset

- **File:** [`data/customer_shopping_behavior.csv`](data/customer_shopping_behavior.csv)
- **Size:** 3,900 rows x 18 columns
- **Missing values:** 37 in `Review Rating` (filled with the median); no duplicate rows

| Column | Description |
|---|---|
| `Customer ID` | Unique customer identifier |
| `Age` | Customer age (18 to 70) |
| `Gender` | Male / Female |
| `Item Purchased` | Product bought (25 unique items) |
| `Category` | Clothing, Accessories, Footwear, Outerwear |
| `Purchase Amount (USD)` | Amount spent per purchase |
| `Location` | US state (50 unique) |
| `Size` | S, M, L, XL |
| `Color` | Product color (25 unique) |
| `Season` | Spring, Summer, Fall, Winter |
| `Review Rating` | Rating from 2.5 to 5.0 |
| `Subscription Status` | Yes / No |
| `Shipping Type` | Free, Standard, Express, 2-Day, Next Day Air, Store Pickup |
| `Discount Applied` | Yes / No |
| `Promo Code Used` | Yes / No |
| `Previous Purchases` | Count of earlier purchases |
| `Payment Method` | PayPal, Credit Card, Cash, Debit Card, Venmo, Bank Transfer |
| `Frequency of Purchases` | Weekly, Bi-Weekly, Fortnightly, Monthly, Quarterly, Every 3 Months, Annually |

---

## Tools

| Purpose | Tool |
|---|---|
| Data loading, cleaning, EDA | Python (Pandas, NumPy, Matplotlib, Seaborn) |
| Database and SQL analysis | PostgreSQL |
| Dashboard and reporting | Power BI (DAX, slicers) |
| Version control | Git, GitHub |

---

## Steps

1. **Load data:** read the CSV into a Pandas DataFrame.
2. **EDA:** check structure, data types, missing values, duplicates, and distributions of age, purchase amount, and ratings.
3. **Clean data:** standardize column names to snake_case (e.g., `purchase_amount`), fill the 37 missing review ratings with the median, and create an `age_group` column with four quartile-based groups: young_adult (18 to 31), adult (32 to 44), middle_aged (45 to 57), and senior (58 to 70).
4. **Load to PostgreSQL:** write the cleaned data into a `customer` table using SQLAlchemy.
5. **SQL analysis:** query revenue and customer counts by category, age group, subscription status, and shipping type.
6. **Build dashboard:** connect Power BI to PostgreSQL, create DAX measures, KPI cards, charts, and slicers.

**Sample SQL**

```sql
-- Revenue and customers by category
SELECT category,
       COUNT(customer_id)     AS customers,
       SUM(purchase_amount)   AS total_revenue,
       ROUND(AVG(purchase_amount), 2) AS avg_purchase
FROM customer
GROUP BY category
ORDER BY total_revenue DESC;

-- Subscribers vs non-subscribers
SELECT subscription_status,
       COUNT(*)               AS customers,
       ROUND(AVG(purchase_amount), 2) AS avg_purchase,
       ROUND(AVG(review_rating), 2)   AS avg_rating
FROM customer
GROUP BY subscription_status;
```

---

## Dashboard

**KPI cards**
- Number of customers: **3,900**
- Average purchase amount: **$59.76**
- Average review rating: **3.75**

**Visuals**
- Subscription status split (donut chart)
- Revenue by category and sales by category (column charts)
- Revenue by age group and sales by age group (bar charts)

**Slicers:** subscription status, gender, category, shipping type

![Customer Behavior Dashboard](images/dashboard.png)

---

## Results

**Total revenue: $233,081** across 3,900 customers.

| Insight | Finding |
|---|---|
| Top category | **Clothing**: $104,264 (44.7% of revenue), followed by Accessories ($74,200) |
| Top age group | **young_adult (18 to 31)**: $62,143 (26.7% of revenue) with the most customers (1,028); the other groups are close behind: middle_aged $59,197, adult $55,978, senior $55,763 |
| Subscribers | 1,053 customers (27%); average spend $59.49 vs $59.87 for non-subscribers, nearly identical |
| Gender | Male customers generate 67.7% of revenue ($157,890); average spend is similar (M $59.54, F $60.25) |
| Shipping | Free Shipping has the highest revenue ($40,777); 2-Day Shipping has the highest average purchase ($60.73) |
| Season | Fall is the strongest season ($60,018) |
| Top items | Blouse ($10,410), Shirt ($10,332), Dress ($10,320) |
| Top state | Montana ($5,784), then Illinois and California |
| Discounts | Average purchase is slightly lower with a discount ($59.28 vs $60.13) |
| Ratings | Average 3.75; consistent across categories (3.72 to 3.79) |

**Recommendations**
- Prioritize Clothing and Accessories in inventory and promotions; together they bring 76% of revenue.
- Focus campaigns on young adults (18 to 31), the biggest revenue segment, while keeping offers for the other age groups since revenue is fairly evenly spread.
- Subscribers do not spend more per purchase, so add subscriber-only perks to increase their value.
- Discounts do not raise basket size, so use them selectively.
- Grow the female customer segment, which is only 32% of customers.

---

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/hSujal-1/Customer_behavior_analytics.git
cd Customer_behavior_analytics

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn sqlalchemy psycopg2-binary jupyter

# 3. Run the notebook (EDA, cleaning, load to PostgreSQL)
jupyter notebook notebooks/analysis.ipynb
```

4. Create a PostgreSQL database named `customer_db`.
5. Update the database credentials in the notebook, then run it to create the `customer` table.
6. Run the queries in `sql/analysis_queries.sql` to reproduce the analysis.
7. Open `dashboard/Customer_Behavior-Dashboard.pbix` in Power BI Desktop, update the PostgreSQL connection (server, database, credentials), and click **Refresh**.

---

## Project Structure

```
├── data/
│   └── customer_shopping_behavior.csv
├── notebooks/
│   └── analysis.ipynb
├── sql/
│   └── analysis_queries.sql
├── dashboard/
│   └── Customer_Behavior-Dashboard.pbix
├── images/
│   └── dashboard.png
└── README.md
```

---

## Author

**Sujal Jambotkar**
[LinkedIn](https://linkedin.com/in/sujaljambotkar11) | [GitHub](https://github.com/hSujal-1) | sujaljambotkar11@gmail.com

## License

This project is licensed under the MIT License.
