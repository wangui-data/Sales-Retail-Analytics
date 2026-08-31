# retail-sales-analytics

# 🧸 Maven Toys: Retail Sales & Inventory Analysis

**Power BI** · **DAX** · **Power Query** · **Data Modeling** · **Business Intelligence**

---

## 📖 Project Overview

Maven Toys is a  toy retailer operating across multiple stores and product categories.

This project analyzes **retail sales and inventory performance** using Microsoft Power BI to understand how products and stores are performing, how revenue and profitability are changing over time, and whether current inventory levels are aligned with sales performance.

The project follows an end-to-end business intelligence workflow:

**Raw Data → Power Query → Data Model → DAX → Analysis → Interactive Dashboard → Business Recommendations**

Rather than relying on pre-calculated metrics, key business KPIs are dynamically calculated using DAX measures within Power BI.

---

# 🎯 Business Problem

Maven Toys needs better visibility into its retail performance in order to make informed decisions around **sales, product profitability, store performance, and inventory management**.

The analysis focuses on five key business areas.

### 1️⃣ Sales Performance

How is the business performing over time?

* What is total revenue?
* How many units are being sold?
* How many sales transactions are occurring?
* How are sales changing over time?
* Which stores generate the most revenue?

---

### 2️⃣ Product Performance

Which products and categories are driving the business?

* Which products generate the most revenue?
* Which products sell the most units?
* Which categories are most profitable?
* Which products have high sales but low margins?
* Which products contribute most to overall revenue?

---

### 3️⃣ Store Performance

How does performance differ across Maven Toys locations?

* Which stores generate the most revenue?
* Which stores generate the most profit?
* Which locations sell the highest number of units?
* How does inventory vary between stores?
* Are high-revenue stores also highly profitable?

---

### 4️⃣ Inventory Performance

Is inventory aligned with sales demand?

* How much stock is currently available?
* Which products have the highest inventory levels?
* Which stores hold the most inventory?
* Which products have high stock but relatively low sales?
* Which products may require additional stock based on sales performance?

---

### 5️⃣ Profitability

Are high sales translating into profitable performance?

* What is total profit?
* What is the overall profit margin?
* Which products generate the highest profit?
* Which categories have the strongest margins?
* Which stores contribute most to profitability?

---

# 🚀 Project Objectives

✔ Build an interactive Power BI retail dashboard.

✔ Clean and transform raw data using Power Query.

✔ Build a structured relational data model.

✔ Apply fact and dimension table concepts.

✔ Establish appropriate one-to-many relationships.

✔ Create a dedicated calendar table for time-based analysis.

✔ Develop reusable DAX measures for sales, revenue, cost, profit, and inventory.

✔ Analyze sales trends over time.

✔ Evaluate product and category performance.

✔ Compare store performance.

✔ Analyze current inventory levels.

✔ Identify potential inventory and profitability opportunities.

✔ Translate analytical findings into business recommendations.

---

# 🛠 Tools Used

| Tool            | Purpose                                             |
| --------------- | --------------------------------------------------- |
| **Power BI**    | Data modeling, visualization, dashboard development |
| **Power Query** | Data cleaning and transformation                    |
| **DAX**         | Dynamic measures and business calculations          |
| **GitHub**      | Project documentation and portfolio presentation    |

---

# 📂 Dataset Overview

The Maven Toys dataset contains multiple tables representing different aspects of the retailer's operations.

The project uses five main tables:

### `sales`

Contains individual retail sales transactions.

| Column       | Description                         |
| ------------ | ----------------------------------- |
| `Sale_ID`    | Unique sales transaction identifier |
| `Date`       | Transaction date                    |
| `Product_ID` | Product identifier                  |
| `Store_ID`   | Store identifier                    |
| `Units`      | Number of units sold                |

---

### `products`

Contains information about products sold by Maven Toys.

| Column             | Description               |
| ------------------ | ------------------------- |
| `Product_ID`       | Unique product identifier |
| `Product_Name`     | Product name              |
| `Product_Category` | Product category          |
| `Product_Cost`     | Cost per unit             |
| `Product_Price`    | Selling price per unit    |

---

### `stores`

Contains information about Maven Toys stores.

| Column            | Description                   |
| ----------------- | ----------------------------- |
| `Store_ID`        | Unique store identifier       |
| `Store_Name`      | Store name                    |
| `Store_City`      | City where the store operates |
| `Store_Location`  | Store location/region         |
| `Store_Open_Date` | Store opening date            |

---

### `inventory`

Contains current inventory information.

| Column          | Description             |
| --------------- | ----------------------- |
| `Product_ID`    | Product identifier      |
| `Store_ID`      | Store identifier        |
| `Stock_On_Hand` | Current units available |

---

### `calendar`

A dedicated date dimension used for time-based analysis.

| Column       | Description   |
| ------------ | ------------- |
| `Date`       | Calendar date |
| `Month Name` | Month name    |
| `Year`       | Calendar year |

---

# 📖 Data Model

The Power BI model uses a **star-schema-inspired design with two fact tables**.

### Fact Tables

* `sales`
* `inventory`

### Dimension Tables

* `calendar`
* `products`
* `stores`

The `products` and `stores` tables act as shared dimensions for both sales and inventory analysis.

```text
                         calendar
                            |
                            | 1 : *
                            ↓
                          sales
                         /     \
                    * : 1       1 : *
                       /         \
                      ↓           ↓
                  products      stores
                      1           1
                       \         /
                        \       /
                         ↓     ↓
                       inventory
```

### Relationship Design

| Dimension  | Fact        | Relationship |
| ---------- | ----------- | ------------ |
| `calendar` | `sales`     | 1 : Many     |
| `products` | `sales`     | 1 : Many     |
| `stores`   | `sales`     | 1 : Many     |
| `products` | `inventory` | 1 : Many     |
| `stores`   | `inventory` | 1 : Many     |

The model avoids creating a direct relationship between the two fact tables.

This allows `products` and `stores` to provide common filtering context across sales and inventory analysis.

---

# 🧹 Data Preparation

Power Query was used to prepare the raw data before loading it into the Power BI model.

### Data preparation activities included:

* Reviewing table structures
* Promoting headers
* Removing unnecessary fields
* Checking for duplicate records
* Investigating missing values
* Correcting data types
* Standardizing field names
* Preparing tables for relationships
* Creating and configuring the calendar table

The objective was to create a clean and reliable dataset suitable for analysis.

---

# 🧮 DAX Measures

Key business metrics were created as reusable DAX measures.

## Sales & Revenue

### Total Units Sold

```DAX
Total Units Sold =
SUM(sales[Units])
```

### Total Sales Transactions

```DAX
Total Sales Transactions =
DISTINCTCOUNT(sales[Sale_ID])
```

### Products Sold

```DAX
Products Sold =
DISTINCTCOUNT(sales[Product_ID])
```

### Total Revenue

```DAX
Total Revenue =
SUMX(
    sales,
    sales[Units] * RELATED(products[Product_Price])
)
```

---

## 💰 Profitability

### Total Cost

```DAX
Total Cost =
SUMX(
    sales,
    sales[Units] * RELATED(products[Product_Cost])
)
```

### Total Profit

```DAX
Total Profit =
[Total Revenue] - [Total Cost]
```

### Profit Margin

```DAX
Profit Margin =
DIVIDE(
    [Total Profit],
    [Total Revenue]
)
```

---

## 🛒 Transaction Metrics

### Average Transaction Value

```DAX
Average Transaction Value =
DIVIDE(
    [Total Revenue],
    [Total Sales Transactions]
)
```

### Units per Transaction

```DAX
Units per Transaction =
DIVIDE(
    [Total Units Sold],
    [Total Sales Transactions]
)
```

### Average Selling Price

```DAX
Average Selling Price =
DIVIDE(
    [Total Revenue],
    [Total Units Sold]
)
```

---

## 📦 Inventory

### Current Stock

```DAX
Current Stock =
SUM(inventory[Stock_On_Hand])
```

### Inventory Value

```DAX
Inventory Value =
SUMX(
    inventory,
    inventory[Stock_On_Hand] *
    RELATED(products[Product_Cost])
)
```

The inventory value represents the estimated cost value of the current stock held.

---

# ❓ Key Business Questions

## Sales Performance

* What is total revenue?
* How many units have been sold?
* How many transactions have occurred?
* How do sales change over time?
* Which stores generate the most revenue?
* Which product categories generate the most revenue?

---

## Product Performance

* Which products generate the most revenue?
* Which products sell the most units?
* Which products generate the most profit?
* Which categories have the highest margins?
* Which products have high sales but relatively low profitability?
* Which products contribute the most to total revenue?

---

## Store Performance

* Which stores are the strongest performers?
* Which stores generate the most revenue?
* Which stores generate the most profit?
* Which locations sell the most units?
* How does inventory differ between stores?

---

## Inventory Performance

* How much inventory is currently held?
* Which products have the highest stock levels?
* Which stores hold the most inventory?
* Which products have high stock but relatively low sales?
* Which products may require additional inventory based on demand?

---

## Profitability

* How much profit does Maven Toys generate?
* What is the overall profit margin?
* Which products contribute most to profit?
* Which categories have the strongest margins?
* Are high-revenue products also highly profitable?

---

# 📐 KPI Definitions

| Metric                        | Formula                      |
| ----------------------------- | ---------------------------- |
| **Revenue**                   | Units Sold × Product Price   |
| **Cost**                      | Units Sold × Product Cost    |
| **Profit**                    | Revenue − Cost               |
| **Profit Margin**             | Profit ÷ Revenue             |
| **Average Transaction Value** | Revenue ÷ Transactions       |
| **Units per Transaction**     | Units Sold ÷ Transactions    |
| **Inventory Value**           | Stock on Hand × Product Cost |

---

# 📊 Dashboard Structure

The Power BI report is organized into multiple analytical pages.

## 1️⃣ Executive Overview

Provides a high-level view of Maven Toys' overall performance.

### KPIs

* Total Revenue
* Total Profit
* Profit Margin
* Total Units Sold
* Sales Transactions
* Current Stock

### Analysis

* Sales trend
* Revenue by category
* Revenue by store
* Profit by category
* Top-performing products

---

## 2️⃣ Sales Performance

Focuses on the company's sales activity.

### Analysis

* Revenue trends
* Units sold
* Transactions
* Sales by product category
* Sales by store
* Sales by location

---

## 3️⃣ Product Performance

Focuses on product and category-level performance.

### Analysis

* Top products
* Bottom products
* Revenue by category
* Profit by product
* Profit margin by product
* Units sold by product

---

## 4️⃣ Store Performance

Focuses on store and location performance.

### Analysis

* Revenue by store
* Profit by store
* Units sold by store
* Store/location comparisons
* Inventory by store

---

## 5️⃣ Inventory Performance

Focuses on current inventory levels and their relationship with sales.

### Analysis

* Current stock
* Inventory value
* Stock by product
* Stock by store
* High-stock/low-sales products
* Inventory concentration

---

# 🎛️ Dashboard Interactivity

The Power BI report incorporates interactive features to allow users to explore the data dynamically.

### Filters & Slicers

Users can filter the analysis by:

* Year
* Month
* Product Category
* Product
* Store
* Location

### Interactive Analysis

The dashboard is designed to allow users to move from high-level KPIs into product, store, and inventory-level analysis.

---

# 🔍 Analytical Approach

The analysis follows a structured process:

```text
Raw Retail Data
       ↓
Power Query
       ↓
Data Cleaning
       ↓
Data Modeling
       ↓
Relationships
       ↓
DAX Measures
       ↓
Data Validation
       ↓
Exploratory Analysis
       ↓
Dashboard Development
       ↓
Business Insights
       ↓
Recommendations
```

---

# 💡 Business Recommendations

Recommendations will be based on the findings identified through the Power BI analysis.

Potential areas of action include:

### 1️⃣ Optimize Inventory

Identify products with relatively high inventory and weak sales performance to reduce potential overstocking.

### 2️⃣ Protect High-Demand Products

Ensure products with strong sales performance maintain sufficient stock availability.

### 3️⃣ Prioritize Profitable Products

Focus on products and categories that combine strong sales volume with healthy profit margins.

### 4️⃣ Improve Store-Level Allocation

Use store-level sales and inventory patterns to identify opportunities for more efficient inventory distribution.

### 5️⃣ Monitor Revenue vs Profit

Management should evaluate both revenue and profitability rather than using sales volume alone to determine product or store performance.

---

# 📌 Expected Insights

The completed analysis will provide visibility into:

* Overall retail sales performance
* Revenue and profit trends
* Top-performing products
* Underperforming products
* Category profitability
* Store performance
* Inventory distribution
* Potential overstocking
* High-demand products
* Opportunities for improved inventory allocation

---

# 📊 Deliverables

* ✔ Power BI data model
* ✔ Power Query data transformation
* ✔ DAX measure library
* ✔ Interactive Power BI dashboard
* ✔ Sales performance analysis
* ✔ Product performance analysis
* ✔ Store performance analysis
* ✔ Inventory analysis
* ✔ Business insights
* ✔ Executive recommendations

---

# 🚀 Skills Demonstrated

### Data Preparation

* Power Query
* Data cleaning
* Data transformation
* Data validation

### Data Modeling

* Fact and dimension tables
* Star schema concepts
* One-to-many relationships
* Multiple fact tables
* Date dimension
* Relationship management

### DAX

* `SUM`
* `SUMX`
* `DISTINCTCOUNT`
* `RELATED`
* `DIVIDE`
* `CALCULATE`
* Time intelligence

### Power BI

* KPI cards
* Interactive dashboards
* Slicers
* Trend analysis
* Product analysis
* Store analysis
* Inventory analysis
* Data storytelling

### Business Analytics

* Sales performance
* Profitability analysis
* Product performance
* Store performance
* Inventory analysis
* Business recommendations

---

# 👤 Author

**Wangui Esther**

Data Analyst | Power BI | Excel | SQL | Business Intelligence

---

## 📌 Project Status

🚧 **In Progress**

The dashboard, DAX measures, business insights, and recommendations are currently being developed.
