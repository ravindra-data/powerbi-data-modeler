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
| Report Page | Page 1 |
| Schema Type | Star Schema |
| Total Visuals | 3 Matrix (Pivot) Tables |

---

## 🗃️ Data Model — 6 Tables

| Table | Type | Role in Model |
|---|---|---|
| Sales Data | Fact Table | Core transactional data — Revenue, Quantity, Discount |
| Return Data | Fact Table | Returns transactions — ReturnID, Reason, Return dates |
| Customer Data | Dimension Table | Customer profiles — CustomerID, Segment, demographics |
| Product Data | Dimension Table | Product info — Category, Subcategory, ProductName |
| Region Data | Dimension Table | Geographic mapping — Country, State, City |
| Date Data | Dimension Table | Date intelligence — Date, Month, Quarter, Year, Fiscal Year |

---

## 🔗 Relationships & Cardinality

| From Table (Dimension) | Key Column | To Table (Fact) | Cardinality | Cross-Filter | Status |
|---|---|---|---|---|---|
| Customer Data | CustomerID | Sales Data | 1 : * | Single | Active |
| Product Data | ProductID | Sales Data | 1 : * | Single | Active |
| Region Data | RegionID | Sales Data | 1 : * | Single | Active |
| Date Data | Date | Sales Data | 1 : * | Single | Active |
| Date Data | Date | Return Data | 1 : * | Single | Inactive |
| Return Data | ReturnID | Sales Data | * : * | Single | Reference |

> **Note:** The Date Data → Return Data relationship on ReturnDate is kept **inactive** (dotted line) to avoid filter ambiguity. It can be activated using `USERELATIONSHIP()` in DAX measures.

---

## 🏷️ Data Categories & Formats

| Table | Column | Data Type | Data Category | Format |
|---|---|---|---|---|
| Region Data | Country | Text | Country/Region | — |
| Region Data | State | Text | State or Province | — |
| Region Data | City | Text | City | — |
| Sales Data | Revenue | Decimal | Uncategorized | Currency |
| Sales Data | Quantity | Whole Number | Uncategorized | Whole Number |
| Date Data | Date | Date | Uncategorized | DD/MM/YYYY |
| Product Data | ProductName | Text | Uncategorized | — |
| Customer Data | Segment | Text | Uncategorized | — |

---

## 📁 Hierarchies Built

| Hierarchy Name | Levels (Top → Bottom) | Table |
|---|---|---|
| Date_Dim Hierarchy | Year > Quarter > Month > Date | Date Data |
| Region_Dim Hierarchy | Country > State > City | Region Data |
| Product_Dim Hierarchy | Category > Subcategory > ProductName | Product Data |

> Hierarchies enable **Drill Down** functionality in visuals. Created via right-click → Create Hierarchy → Add to Hierarchy.

---

## 📊 Matrix Visual Verification (Page 1)

### Visual 1 — Sales grouped by Product Category and Region
| Field | Column Used |
|---|---|
| Rows | Product Data[Category] |
| Columns | Region Data[Country] |
| Values | Sum of Sales Data[Revenue] |

### Visual 2 — Return Reasons by Fiscal Year
| Field | Column Used |
|---|---|
| Rows | Return Data[Reason] |
| Columns | Date Data[Fiscal Year] |
| Values | Count of Return Data[ReturnID] |

### Visual 3 — Revenue by Customer Segment
| Field | Column Used |
|---|---|
| Rows | Customer Data[Segment] |
| Columns | *(Empty)* |
| Values | Sum of Sales Data[Revenue] |

---

## 🚧 Challenges & Solutions

| Challenge | Solution |
|---|---|
| Inactive relationship causing filter ambiguity | Kept ReturnDate relationship inactive; use USERELATIONSHIP() in DAX to activate when needed |
| Bidirectional filters causing circular paths | Set all relationships to Single cross-filter direction |
| Many-to-Many ambiguity between Return & Sales Data | Reviewed cardinality and adjusted join key |
| Data Categories not set for geographic columns | Manually set Country/Region, State or Province, City via Column Tools tab |
| Hierarchies missing for drill-down | Built Date, Region and Product hierarchies via right-click > Create Hierarchy |

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
├── screenshots/
│   └── report.png                        # Report preview
└── README.md                             # Project documentation
```

---

## 💡 What I Learned

- Building a **Star Schema** data model with Fact and Dimension tables
- Configuring **Relationship Cardinality** (1:*, *:*) and **Cross-filter Directions**
- Managing **Active and Inactive Relationships** to avoid filter ambiguity
- Setting **Data Categories** for geographic columns for map visuals
- Creating **Hierarchies** for drill-down functionality in visuals
- Verifying the data model using **Matrix (Pivot) Table** visuals

---
