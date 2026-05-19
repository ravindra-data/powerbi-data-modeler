# 🗂️ Data Modeler — Power BI Data Modelling Project

A comprehensive Power BI project demonstrating end-to-end data modelling — building a Star Schema with relationships, hierarchies, data categories and matrix visual verification across 6 tables covering sales, returns, customers, products, regions and dates.

---

## 📌 Project Overview

| Property | Details |
|---|---|
| Tool | Microsoft Power BI Desktop |
| File | Data_Modeler.pbix |
| Student | Ravindra Kumar |
| GR ID | 11638 |
| Total Tables | 6 (2 Fact + 4 Dimension) |
| Total Records | 1,680 rows across all tables |
| Date Range | FY2022 – FY2023 (2 Fiscal Years) |
| Report Page | Page 1 |
| Schema Type | Star Schema |
| Total Visuals | 3 Matrix (Pivot) Tables |

---

## 🗃️ Data Model — 6 Tables

| Table | Type | Rows | Key Column | Role in Model |
|---|---|---|---|---|
| Sales_Fact | Fact Table | 500 | SalesID | Core transactional data — Revenue, Quantity, Discount |
| Returns_Fact | Fact Table | 100 | ReturnID | Returns transactions — linked to Sales via SalesID |
| Customer_Dim | Dimension | 200 | CustomerID | Customer profiles — Segment, Age, Gender |
| Product_Dim | Dimension | 100 | ProductID | Product info — Category, Subcategory, Brand |
| Region_Dim | Dimension | 50 | RegionID | Geographic mapping — Country, State, City |
| Date_Dim | Dimension | 730 | DateKey | Date intelligence — Month, Quarter, Year, Fiscal Year |

---

## 📋 Dataset Details

### 🧾 Sales_Fact (500 rows)
| Column | Data Type | Description |
|---|---|---|
| SalesID | Whole Number | Primary Key — unique transaction ID |
| CustomerID | Whole Number | Foreign Key → Customer_Dim |
| ProductID | Whole Number | Foreign Key → Product_Dim |
| RegionID | Whole Number | Foreign Key → Region_Dim |
| DateKey | Whole Number | Foreign Key → Date_Dim |
| Quantity | Whole Number | Units sold (range: 1 – 9) |
| Revenue | Decimal Number | Sale amount in currency (range: 103 – 996) |
| Discount | Whole Number | Discount % applied (0%, 5%, 10%, 15%, 20%) |

### 🔄 Returns_Fact (100 rows)
| Column | Data Type | Description |
|---|---|---|
| ReturnID | Whole Number | Primary Key — unique return ID |
| SalesID | Whole Number | Foreign Key → Sales_Fact |
| ReturnDateKey | Whole Number | Foreign Key → Date_Dim (Inactive relationship) |
| Reason | Text | Return reason: Damaged / Not Needed / Wrong Item |

### 👤 Customer_Dim (200 rows)
| Column | Data Type | Description |
|---|---|---|
| CustomerID | Whole Number | Primary Key |
| FullName | Text | Customer full name |
| Age | Whole Number | Age range: 18 – 64 |
| Gender | Text | Male / Female |
| Segment | Text | Gold / Silver / Platinum |

### 📦 Product_Dim (100 rows)
| Column | Data Type | Description |
|---|---|---|
| ProductID | Whole Number | Primary Key |
| ProductName | Text | Product name |
| Category | Text | Clothing / Electronics / Furniture |
| Subcategory | Text | Jeans / Laptop / Mobile / Shirt / Sofa / Table |
| Brand | Text | Brand A / Brand B / Brand C |

### 🌍 Region_Dim (50 rows)
| Column | Data Type | Description | Data Category |
|---|---|---|---|
| RegionID | Whole Number | Primary Key | — |
| Country | Text | USA / India / Germany | Country/Region |
| State | Text | State code (CA, DL, MH, NY, BW, TX) | State or Province |
| City | Text | City name | City |

### 📅 Date_Dim (730 rows)
| Column | Data Type | Description |
|---|---|---|
| DateKey | Whole Number | Primary Key (format: YYYYMMDD) |
| Date | Date | Full date value |
| Month | Whole Number | Month number (1–12) |
| Quarter | Whole Number | Quarter number (1–4) |
| Year | Whole Number | 2022 / 2023 |
| Fiscal Year | Text | FY2022 / FY2023 |

---

## 🔗 Relationships & Cardinality

| From Table (Dimension) | Key Column | To Table (Fact) | Cardinality | Cross-Filter | Status |
|---|---|---|---|---|---|
| Customer_Dim | CustomerID | Sales_Fact | 1 : * | Single | ✅ Active |
| Product_Dim | ProductID | Sales_Fact | 1 : * | Single | ✅ Active |
| Region_Dim | RegionID | Sales_Fact | 1 : * | Single | ✅ Active |
| Date_Dim | DateKey | Sales_Fact | 1 : * | Single | ✅ Active |
| Date_Dim | DateKey | Returns_Fact | 1 : * | Single | ⚪ Inactive |
| Returns_Fact | SalesID | Sales_Fact | * : * | Single | 🔗 Reference |

> **Note:** The Date_Dim → Returns_Fact relationship on ReturnDateKey is kept **inactive** (dotted line) to avoid filter ambiguity. Activate using `USERELATIONSHIP()` in DAX when needed.

---

## 📁 Hierarchies Built

| Hierarchy Name | Levels (Top → Bottom) | Table |
|---|---|---|
| Date_Dim Hierarchy | Year > Quarter > Month > Date | Date_Dim |
| Region_Dim Hierarchy | Country > State > City | Region_Dim |
| Product_Dim Hierarchy | Category > Subcategory > ProductName | Product_Dim |

> Hierarchies enable **Drill Down** functionality in visuals. Created via right-click → Create Hierarchy → Add to Hierarchy.

---

## 🏷️ Data Categories Set

| Table | Column | Data Category |
|---|---|---|
| Region_Dim | Country | Country/Region |
| Region_Dim | State | State or Province |
| Region_Dim | City | City |

---

## 📊 Matrix Visual Verification (Page 1)

### Visual 1 — Sales grouped by Product Category and Region
| Field | Value |
|---|---|
| Rows | Product_Dim[Category] → Clothing, Electronics, Furniture |
| Columns | Region_Dim[Country] → Germany, India, USA |
| Values | Sum of Sales_Fact[Revenue] |

### Visual 2 — Return Reasons by Fiscal Year
| Field | Value |
|---|---|
| Rows | Returns_Fact[Reason] → Damaged, Not Needed, Wrong Item |
| Columns | Date_Dim[Fiscal Year] → FY2022, FY2023 |
| Values | Count of Returns_Fact[ReturnID] |

### Visual 3 — Revenue by Customer Segment
| Field | Value |
|---|---|
| Rows | Customer_Dim[Segment] → Gold, Silver, Platinum |
| Columns | *(Empty — single column layout)* |
| Values | Sum of Sales_Fact[Revenue] |

---

## 🚧 Challenges & Solutions

| Challenge | Solution |
|---|---|
| Inactive relationship between Date_Dim & Returns_Fact causing filter ambiguity | Kept ReturnDateKey relationship inactive; activate using USERELATIONSHIP() in DAX |
| Bidirectional filters causing circular filter paths | Set all relationships to Single cross-filter direction |
| Many-to-Many ambiguity between Returns_Fact and Sales_Fact | Adjusted join key and cardinality; used reference relationship |
| Data Categories not set for geographic columns in Region_Dim | Manually set Country/Region, State or Province, City via Column Tools |
| Hierarchies missing for drill-down in visuals | Built Date, Region and Product hierarchies via right-click > Create Hierarchy |

---

## 🛠️ Tech Stack

- **Tool:** Microsoft Power BI Desktop
- **Feature:** Model View, Column Tools, Report View
- **Operations:** Star Schema Design, Relationship Cardinality, Cross-filter Direction, Data Categories, Hierarchies, Matrix Visuals

---

## 📁 Project Structure

```
powerbi-data-modeler/
│
├── Data_Modeler.pbix                     # Main Power BI file
├── PowerBI_DataModeler_Summary.pdf       # Data model summary report
├── datasets/
│   ├── Customer_Dim.xlsx                 # 200 rows — Customer dimension
│   ├── Date_Dim.xlsx                     # 730 rows — Date dimension (FY2022-FY2023)
│   ├── Product_Dim.xlsx                  # 100 rows — Product dimension
│   ├── Region_Dim.xlsx                   # 50 rows  — Region dimension
│   ├── Returns_Fact.xlsx                 # 100 rows — Returns fact table
│   └── Sales_Fact.xlsx                   # 500 rows — Sales fact table
├── screenshots/
│   └── report.png                        # Report preview
└── README.md                             # Project documentation
```

---

## 💡 What I Learned

- Building a **Star Schema** data model with 2 Fact tables and 4 Dimension tables
- Configuring **Relationship Cardinality** (1:*, *:*) and **Cross-filter Directions**
- Managing **Active and Inactive Relationships** to avoid filter ambiguity
- Setting **Data Categories** for geographic columns (Country, State, City) for map visuals
- Creating **Drill-Down Hierarchies** for Date, Region and Product dimensions
- Verifying the complete data model using **Matrix (Pivot) Table** visuals

---
