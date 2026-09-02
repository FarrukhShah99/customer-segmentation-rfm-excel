# Customer Segmentation with RFM Analysis (Excel)

RFM (Recency, Frequency, Monetary) customer segmentation of the **Global Superstore** dataset, built entirely in Microsoft Excel.

**Analysis date:** 31 December 2014  
**Customers:** 4,873  
**Total sales:** $12,642,905

![Dashboard](images/dashboard.png)

## Project overview

Each customer is scored from 1 (low) to 5 (high) on three metrics, then mapped to a business segment:

| Metric | Meaning | Scoring |
|---|---|---|
| **Recency** | Days since last order | Lower days = higher score |
| **Frequency** | Unique order count | More orders = higher score |
| **Monetary** | Total sales | Higher spend = higher score |

Segments are assigned with an R x F lookup matrix (INDEX/MATCH). Monetary is used to measure value and to rank segments.

## Dataset

[Global Superstore (Kaggle)](https://www.kaggle.com/datasets/abdelazizsami/global-superstore) - AbdelAziz version.

- One row = one order line (not one order)
- Date range used for recency: through **31 Dec 2014**
- Unique customer key: `Customer.ID` (not name - the same name can appear on multiple IDs)

The raw CSV is not stored in this repo. Import it with Power Query as described below.

## Workbook structure

| Sheet | Purpose |
|---|---|
| **Dashboard** | Charts, KPI table, and key insights |
| **Segment Summary** | PivotTable: customer count, sales, and mix by segment |
| **RFM Analysis** | One row per customer, scores, segment, breakpoints, and matrix |
| **Raw Data** | Power Query output (`tblOrders`) |
| **Customer Summary** *(hidden)* | Source PivotTable used to get distinct `Order.ID` counts |

## Method

### 1. Import

CSV imported with **Data > Get Data > From Text/CSV > Transform Data**. `Order.Date` typed as **Date** (the file stores `YYYY-MM-DD 00:00:00.000`; opening the CSV directly in Excel dropped the date and left `00:00`).

### 2. Customer grain

PivotTable on the Data Model with **Distinct Count** of `Order.ID` so Frequency = unique orders, not line items.

| Field | Aggregation |
|---|---|
| Last order date | MAX of `Order.Date` |
| Frequency | Distinct count of `Order.ID` |
| Monetary | SUM of `Sales` |

### 3. Recency

```text
Recency (days) = Analysis Date - Last Order Date
Analysis Date = MAX(Order.Date) = 31 Dec 2014
