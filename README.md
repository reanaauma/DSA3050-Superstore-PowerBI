# DSA3050 – Global Superstore Sales & Profitability Analysis

**Student:** Reana Auma  
**Registration Number:** 669255
**Course:** DSA3050 – Business Intelligence & Data Visualization  
**Project:** Global Superstore Sales & Profitability Analysis

---

## 1. Project Overview

This project presents a Business Intelligence and Data Visualization solution developed in Microsoft Power BI using the **Global Superstore** dataset.

The analysis focuses on understanding:

- Sales performance
- Profitability
- Customer segments
- Product and category performance
- Regional performance
- Discounts
- Shipping performance

The overall objective is to transform transactional sales data into an interactive dashboard that supports evidence-based business decisions.

The BI development process followed the sequence:

**Dataset → Power Query → Data Model → DAX → Dashboard → Insights**

---

## 2. Dataset

### Dataset Name

**Global Superstore**

### Dataset Source

**Source:** Kaggle

### File Name

**File:** superstore.csv

### Dataset Size

- **Rows:** 51,272  
- **Columns:** 26

The loaded Power Query table was named **FactSales**.

### Main Variables

| Variable | Type | Business Meaning |
|---|---|---|
| Order ID | Text/Categorical | Identifies a customer order |
| Order Date | Date | Date the order was placed |
| Ship Date | Date | Date the order was shipped |
| Ship Mode | Categorical | Shipping method |
| Customer ID | Text/Categorical | Identifies a customer |
| Customer Name | Categorical | Customer name |
| Segment | Categorical | Customer segment |
| Country/Region | Categorical | Geographical location |
| City | Categorical | Customer city |
| State/Province | Categorical | Customer state/province |
| Region | Categorical | Business/geographic region |
| Product ID | Text/Categorical | Identifies a product |
| Category | Categorical | Main product category |
| Sub-Category | Categorical | Product sub-category |
| Product Name | Categorical | Product name |
| Sales | Numeric | Sales/revenue amount |
| Quantity | Numeric | Number of units sold |
| Discount | Numeric | Discount applied |
| Profit | Numeric | Profit amount |
| Shipping Cost | Numeric | Shipping cost |

---

## 3. Business Problem

The business needs to understand which products, customer segments and regions generate the most sales and profit while also evaluating discounting and shipping performance.

The analysis is intended to support decisions relating to:

- Product performance
- Customer focus
- Regional strategy
- Discounting
- Profitability
- Operational and shipping efficiency

---

## 4. Analytical Questions

The dashboard was designed to answer the following questions:

1. Which regions generate the highest sales?
2. Which regions generate the highest profit?
3. Which product categories and sub-categories perform best?
4. Which customer segments generate the most sales and profit?
5. Which products generate high sales but relatively low profit?
6. Does discounting appear to be associated with lower profitability?
7. Which shipping modes have the longest average shipping time?
8. Which region/category combinations have weak profitability?

---

# 5. Power Query Data Preparation

Power Query was used to clean, transform and prepare the raw Superstore data before modelling.

The FactSales query contains several meaningful transformation steps.

## 5.1 Correct Data Types

Data types were checked and converted where necessary.

Examples include:

- Order Date and Ship Date → Date
- Sales, Profit, Discount and Shipping Cost → Numeric
- Quantity → Whole Number
- Categorical fields → Text

**Reason:** Correct data types are necessary for accurate calculations, filtering and time analysis.

---

## 5.2 Remove Unnecessary Columns

Unnecessary columns were removed from the FactSales table where they were not required for the analysis.

**Reason:** This reduces unnecessary clutter and keeps the analytical table focused on useful business fields.

---

## 5.3 Trim Text

A **Trimmed Text** transformation was applied to relevant text fields.

**Reason:** Trimming removes unnecessary spaces and improves consistency when grouping, filtering and displaying categorical values.

---

## 5.4 Clean Text

A **Cleaned Text** transformation was applied to relevant text fields.

**Reason:** This improves the consistency and quality of categorical data used in the dashboard.

---

## 5.5 Create Shipping Days

A date subtraction transformation was used to create **Shipping Days** based on the difference between Ship Date and Order Date.

**Concept:**

`Shipping Days = Ship Date - Order Date`

**Reason:** This creates an operational indicator for evaluating shipping performance.

---

## 5.6 Create Sales Category

A conditional column was used to classify sales into meaningful categories.

**Reason:** Sales categories make transaction values easier to compare and analyse.

---

## 5.7 Create Profit Status

A conditional column was used to classify transactions according to profitability.

The categories used include:

- Profitable
- Loss Making

**Reason:** This makes it easier to identify profitable and loss-making transactions.

---

## 5.8 Extract Order Year

The order date was used to derive **Order Year**.

**Reason:** This supports annual sales and profitability trend analysis.

---

## 5.9 Extract Month Name

The order date was used to derive **Month Name**.

**Reason:** This supports monthly trend analysis.

---

## 5.10 Profit Margin

A Profit Margin field was created to provide a profitability-efficiency indicator based on profit relative to sales.

**Concept:**

`Profit Margin = Profit / Sales`

---

## 5.11 Power Query Result

After the transformations, the FactSales table contained cleaned fields and additional analytical fields such as:

- Shipping Days
- Sales Category
- Profit Status
- Order Year
- Month Name
- Profit Margin

The final Power Query state was then loaded into the Power BI data model.

---

# 6. Data Model

The Power BI model was designed around a central **FactSales** table supported by dimension tables.

### Tables Used

- FactSales
- DimCustomer
- DimProduct
- DimLocation
- DimShipMode
- DimDate

### Model Structure

```text
                 DimDate
                    |
                    |
DimCustomer ---- FactSales ---- DimProduct
                    |
                    |
              DimShipMode

              DimLocation
```

### FactSales

FactSales contains transaction-level information including:

- Sales
- Profit
- Quantity
- Discount
- Shipping Cost
- Order information
- Customer information
- Product information
- Location information

### DimCustomer

Contains:

- Customer ID
- Customer Name
- Segment

### DimProduct

Contains:

- Product ID
- Product Name
- Category
- Sub-Category

### DimLocation

Contains geographical information including:

- Country
- Region
- State
- City
- LocationKey

### DimShipMode

Contains:

- Ship Mode

### DimDate

Contains:

- Date
- Year
- Month Number
- Month Name
- Quarter
- Year Month

---

## 6.1 Relationships

The model uses dimension-to-fact relationships with the dimension tables on the **one** side and FactSales on the **many** side.

The main relationships are:

| Dimension | FactSales | Cardinality |
|---|---|---|
| DimCustomer[Customer ID] | FactSales[Customer ID] | 1:* |
| DimProduct[Product ID] | FactSales[Product ID] | 1:* |
| DimShipMode[Ship Mode] | FactSales[Ship Mode] | 1:* |
| DimDate[Date] | FactSales[Order Date] | 1:* |

The intended filter direction is **Single**, from the dimension table to FactSales.

### Location Relationship Note

DimLocation was created and cleaned to provide a unique location key. The model screenshot does not clearly show an active relationship between DimLocation and FactSales, so this relationship should be verified before final submission rather than being assumed to be active.

---

# 7. Date Table

A dedicated Date table was created to support time-based analysis.

The table was based on the minimum and maximum Order Date values in FactSales.

The Date table includes:

- Date
- Year
- Month Number
- Month Name
- Quarter
- Year Month

The Date table was used for the annual sales trend and time-based filtering.

---

# 8. DAX Measures

The following analytical measures were created in FactSales.

### 8.1 Total Sales

```DAX
Total Sales = SUM(FactSales[Sales])
```

### 8.2 Total Profit

```DAX
Total Profit = SUM(FactSales[Profit])
```

### 8.3 Total Orders

```DAX
Total Orders = DISTINCTCOUNT(FactSales[Order ID])
```

### 8.4 Total Customers

```DAX
Total Customers = DISTINCTCOUNT(FactSales[Customer ID])
```

### 8.5 Total Products

```DAX
Total Products = DISTINCTCOUNT(FactSales[Product ID])
```

### 8.6 Total Quantity

```DAX
Total Quantity = SUM(FactSales[Quantity])
```

### 8.7 Average Order Value

```DAX
Average Order Value =
DIVIDE([Total Sales], [Total Orders], 0)
```

### 8.8 Profit Margin

```DAX
Profit Margin =
DIVIDE([Total Profit], [Total Sales], 0)
```

### 8.9 Average Discount

```DAX
Average Discount = AVERAGE(FactSales[Discount])
```

### 8.10 Average Shipping Cost

```DAX
Average Shipping Cost =
AVERAGE(FactSales[Shipping Cost])
```

### 8.11 Average Shipping Days

```DAX
Average Shipping Days =
AVERAGE(FactSales[Shipping Days])
```

### 8.12 Profitable Orders

```DAX
Profitable Orders =
CALCULATE([Total Orders], FactSales[Profit] > 0)
```

### 8.13 Loss Making Orders

```DAX
Loss Making Orders =
CALCULATE([Total Orders], FactSales[Profit] < 0)
```

### 8.14 Profit per Order

```DAX
Profit per Order =
DIVIDE([Total Profit], [Total Orders], 0)
```

### 8.15 Sales per Customer

```DAX
Sales per Customer =
DIVIDE([Total Sales], [Total Customers], 0)
```

### Advanced DAX

Conditional logic using `VAR` and `IF` was also explored for profitability classification.

---

# 9. Dashboard Development

The final Power BI report contains three dashboard pages:

1. Executive Overview
2. Product and Customer Analysis
3. Profitability and Shipping Diagnostics

The pages were designed to allow the user to move from:

**What happened? → Where did it happen? → Why did it happen? → What requires attention?**

---

# 10. Dashboard Page 1 – Executive Overview

### Purpose

The Executive Overview provides a high-level summary of overall sales and profitability performance.

### KPI Cards

The dashboard displays:

- Total Sales
- Total Profit
- Total Orders
- Profit Margin

### Main Visuals

- Total Sales by Year
- Total Sales by Region
- Total Profit by Category
- Region slicer
- Category slicer
- Order Year slicer

### Visible Dashboard Results

From the completed dashboard:

- **Total Sales:** approximately **12.64M**
- **Total Profit:** approximately **1.47M**
- **Total Orders:** approximately **25K**

The annual sales chart shows an overall upward trend across the displayed years.

The regional sales chart shows **Central** as the highest-sales region among the regions visible in the dashboard.

The category profit chart shows **Technology** as the highest-profit category, followed by **Office Supplies** and **Furniture**.

### Profit Margin KPI Note

The current Executive Overview screenshot shows the Profit Margin KPI as **“-Infinity”** and labels it as **“Sum of Profit Margin.”** This indicates that the dashboard's current Profit Margin visual needs to be checked so that it uses the intended Profit Margin measure rather than summing a column.

---

# 11. Dashboard Page 2 – Product and Customer Analysis

### Purpose

This page investigates product performance and customer segment behaviour.

### Visuals

- Product Sales by Sub-Category
- Product Profit by Sub-Category
- Customer Segment sales donut chart
- Customer Profit by Segment
- Top 10 Products by Sales
- Segment and Region filters

### Key Results

The dashboard shows:

- **Phones** as the leading sub-category by sales among the visible products.
- **Copiers** as the leading sub-category by profit among the visible products.
- The **Consumer** segment contributes the largest share of sales.
- Corporate is the second-largest segment.
- Home Office contributes the smallest share of sales among the three segments shown.

The customer profit chart also shows Consumer customers generating the highest total profit, followed by Corporate and Home Office.

---

# 12. Dashboard Page 3 – Profitability and Shipping Diagnostics

### Purpose

This page investigates profitability, discounting and shipping performance.

### Visuals

- Discount vs Profit scatter chart
- Total Profit by Ship Mode
- Average Shipping Days by Ship Mode
- Loss Making Orders KPI
- Profit by Region and Category matrix
- Region slicer
- Category slicer
- Ship Mode slicer

### Visible Results

The dashboard reports approximately:

**8K Loss Making Orders**

The profit matrix shows:

- **Technology** has the highest total profit among the three categories.
- **Office Supplies** is the second-highest.
- **Furniture** has the lowest total profit.

The category totals visible in the matrix are approximately:

| Category | Total Profit |
|---|---:|
| Technology | 663,778.73 |
| Office Supplies | 518,473.83 |
| Furniture | 285,204.72 |
| **Total** | **1,467,457.29** |

The strongest visible region/category combination is:

**Central – Technology: approximately 135,538.42**

The weakest visible region/category combination is:

**Southeast Asia – Furniture: approximately -7,269.77**

This indicates that some region/category combinations can be profitable while others generate losses and therefore require closer monitoring.

---

# 13. Business Insights

## Insight 1 – Strong Overall Sales and Profit

**Finding:** The business generates approximately 12.64M in sales and 1.47M in profit.

**Evidence:** Executive Overview KPI cards.

**Business implication:** The overall business is generating positive profit while maintaining substantial sales volume.

**Recommendation:** Continue monitoring the drivers of profitability, particularly category, region and discount performance.

---

## Insight 2 – Technology is the Most Profitable Category

**Finding:** Technology generates the highest total profit among the three categories.

**Evidence:** Total Profit by Category and the profitability matrix show Technology at approximately 663,778.73.

**Business implication:** Technology is a major contributor to overall profitability.

**Recommendation:** Continue supporting strong-performing Technology products while investigating opportunities to improve the profitability of weaker categories.

---

## Insight 3 – Central Region Performs Strongly

**Finding:** Central is the highest-sales region visible in the Executive Overview and also has the highest total profit in the profitability matrix.

**Evidence:** Regional sales chart and region/category profitability matrix.

**Business implication:** Central is an important contributor to business performance.

**Recommendation:** Analyse the products and customer segments driving Central's performance and consider whether successful practices can be replicated in weaker regions.

---

## Insight 4 – Consumer Segment is the Largest

**Finding:** Consumer customers account for the largest share of sales among the three customer segments.

**Evidence:** Customer Segment donut chart.

**Business implication:** Consumer customers represent an important source of revenue and profit.

**Recommendation:** Maintain strong engagement with Consumer customers while identifying opportunities to grow the Corporate and Home Office segments.

---

## Insight 5 – Some Region/Category Combinations Require Attention

**Finding:** Profitability varies substantially across region/category combinations.

**Evidence:** The profitability matrix shows positive and negative values, including a negative result for Southeast Asia – Furniture.

**Business implication:** A category that performs well overall can still perform poorly in specific regions.

**Recommendation:** Review pricing, discounts, demand and operating costs for weak region/category combinations before increasing investment in those areas.

---

# 14. Dashboard Limitations and Quality Checks

The current dashboard provides useful analytical views, but a few items should be verified before final submission.

### Profit Margin

The Executive Overview currently displays **-Infinity** for Profit Margin and labels the value as **Sum of Profit Margin**. The intended measure should calculate:

`Total Profit / Total Sales`

The visual should therefore be checked to ensure it uses the Profit Margin measure.

### Shipping Days Visual

The Average Shipping Days chart in the screenshot displays the values on a percentage-style 0–100% scale. This should be checked so that shipping days are displayed as a numeric number of days rather than as percentages.

### DimLocation

DimLocation exists in the model, but the submitted model screenshot does not clearly show an active relationship to FactSales. The relationship should be verified if location filtering is expected to propagate through the model.

These checks are important because the assignment emphasizes accurate modelling, sensible number formats, working relationships and meaningful dashboard visuals.

---

# 15. Screenshots

The repository contains evidence of the BI development process.

| Screenshot | Description |
|---|---|
| `01_raw_data.png` | Initial Global Superstore dataset loaded in Excel/Power Query |
| `02_power_query.png` | Power Query transformations and cleaned FactSales table |
| `03_model.png` | Power BI data model showing FactSales and dimension tables |
| `04_dax_measures1.png` | DAX measures and calculations |
| `04_dax_measures2.png` | Additional DAX measures |
| `04_dax_measures3.png` | Additional measures/calculated fields |
| `04_dax_measures4.png` | Additional measures and calculated fields |
| `05_dashboard_overview.png` | Executive Overview dashboard |
| `06_dashboard_analysis.png` | Product and Customer Analysis dashboard |
| `07_dashboard_analysis.png` | Profitability and Shipping Diagnostics dashboard |

---

# 16. Repository Structure

The recommended GitHub repository structure is:

```text
DSA3050-PowerBI-Reana-Auma-RegNo/
│
├── README.md
│
├── data/
│   └── superstore.csv
│
├── powerbi/
│   └── DSA3050_Superstore_Reana_Auma.pbix
│
└── screenshots/
    ├── 01_raw_data.png
    ├── 02_power_query.png
    ├── 03_model.png
    ├── 04_dax_measures1.png
    ├── 04_dax_measures2.png
    ├── 04_dax_measures3.png
    ├── 04_dax_measures4.png
    ├── 05_dashboard_overview.png
    ├── 06_dashboard_analysis.png
    └── 07_dashboard_analysis.png
```

---

# 17. Suggested Git Commit History

Meaningful commits can be used to show the progression of the project:

1. `Added Superstore dataset and project structure`
2. `Completed Power Query transformations`
3. `Created star schema data model`
4. `Created DimDate and relationships`
5. `Added DAX business measures`
6. `Created executive dashboard`
7. `Added product and customer analysis`
8. `Added profitability and shipping diagnostics`
9. `Completed README and documentation`

---

# 18. Conclusion

The Global Superstore Power BI project transformed raw transactional data into a structured Business Intelligence solution.

Power Query was used to clean and transform the data, a dimensional data model was created to support analysis, DAX measures were developed for key business metrics, and three interactive dashboard pages were created.

The analysis indicates that:

- The business generates approximately 12.64M in sales.
- Total profit is approximately 1.47M.
- Technology is the strongest overall profit-generating category.
- Central is a strong-performing region.
- Consumer customers contribute the largest share of sales and profit.
- Approximately 8K orders are identified as loss making.
- Profitability varies considerably by region and category.

The dashboard therefore provides management with a structured way to monitor sales, profitability, customers, products and operational performance and to identify areas requiring further investigation.

---

## 19. Final Submission Checklist

- [ ] Dataset included
- [ ] Dataset row count confirmed
- [ ] 26 columns documented
- [ ] At least 8 meaningful Power Query transformations documented
- [ ] Power Query screenshot included
- [ ] FactSales and dimension tables included
- [ ] Relationships checked
- [ ] Dedicated Date table created and marked
- [ ] DAX measures included
- [ ] Three dashboard pages completed
- [ ] Slicers/interactivity included
- [ ] Dashboard formatting checked
- [ ] Profit Margin KPI verified
- [ ] Shipping Days visual verified
- [ ] DimLocation relationship verified
- [ ] README.md included
- [ ] PBIX file included
- [ ] Dataset/source file included
- [ ] All screenshots included
