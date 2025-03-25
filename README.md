# 📊 E-Commerce Real-Time Monitoring Dashboard (Splunk)

This project is a beginner-friendly **Splunk dashboard** for real-time monitoring and analysis of e-commerce data. It visualizes product trends, error mentions, discount insights, and stock availability using a dataset from Kaggle.

---

## 🔍 Features

- 📈 **Product Trends** – Track the number of products added over time
- 🔴 **Error Tracking** – Find product listings mentioning errors like "Error 496"
- 💰 **Top Discounts** – List products with the highest discount percentages
- ⭐ **Ratings Overview** – Analyze average ratings and distributions
- 🛍️ **Stock Status** – Identify out-of-stock products and track counts

---

## 🧰 Tools Used

- Splunk Enterprise (local instance)
- SPL (Search Processing Language)
- Kaggle E-Commerce Product Data (CSV)
- Basic data cleaning (in Splunk)
- Dashboard panels and visualizations

- ## 📁 Project Files

| File | Description |
|------|-------------|
| `ecommerce_sample_data.xlsx` | Sample data used for uploading into Splunk |
| `search_queries.txt` | All SPL queries used in this project |
| `screenshots/` | Folder with all dashboard screenshots |
| `README.md` | Project documentation |

---

## 📜 Sample SPL Queries
1.	Product Sales
host="ecommerce data"
| timechart span=1d count as ProductCount

2.	Error Tracking: 
host="ecommerce data" "error"
| timechart count

3.	Top Discounted Items:
host="ecommerce data"
| eval actual=tonumber(replace(actual_price, ",", ""))
| eval selling=tonumber(selling_price)
| eval discount_percent=((actual - selling) / actual) * 100
| sort - discount_percent
| table title brand actual_price selling_price discount_percent


4.	Ratings Overview: 
host="ecommerce data"
| eval rating=tonumber(average_rating)
| stats avg(rating) as AvgRating by brand
| sort -AvgRating

Rating Distribution:
host="ecommerce data"
| eval rating=tonumber(average_rating)
| stats count by rating
| sort rating


5.	Products Out of Stock
host="ecommerce data" out_of_stock=true
| stats count as OutOfStockCount
