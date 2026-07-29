# IT-Tech-Sales-Model
Solving practical business problems with DAX requires a solid grasp of Row Context vs. Filter Context, dynamic aggregations, and data auditing. Breakdown of 10 essential DAX patterns I recently worked through:  Iterators (SUMX, COUNTX): (Quantity * UnitPrice), Data Segmentation, Filter Context (CALCULATE), Data Quality Audits, Measure Branching.

# 📊 Power BI Analytics: Mid-Level DAX Business Scenarios

## 📌 Project Overview

This project demonstrates the implementation of **Data Analysis Expressions (DAX)** in Power BI to solve 10 real-world business scenarios. It covers critical business intelligence concepts including row-level verification, dynamic data segmentation, multi-condition filtering, data auditing, and measure branching.

---

## 💡 Summary of Implemented DAX Scenarios

### Scenario 1: Verifying Data with Iterators (`SUMX`)

* **Objective:** Verify source system totals by calculating line-item revenue row-by-row.
* **DAX:**
Verified Revenue = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])

---

### Scenario 2: Categorizing Data (`IF` - Calculated Column)

* **Objective:** Segment transactions into order size categories for marketing analysis.
* **DAX:**
Order Size = IF(Sales[Quantity] > 50, "Bulk Order", "Standard Order")

---

### Scenario 3: Conditional Counting (`CALCULATE`, `COUNTROWS`)

* **Objective:** Count transaction volume for specific high-performing regions (e.g., Maharashtra).
* **DAX:**
Maharashtra Transactions = CALCULATE(COUNTROWS(Sales), Sales[State] = "Maharashtra")

---

### Scenario 4: Handling Text and Blanks (`COUNTA`)

* **Objective:** Audit CRM data quality by counting non-blank customer name records.
* **DAX:**
Valid Customer Count = COUNTA(Sales[Customer Name])

---

### Scenario 5: Multi-Condition Filtering (`CALCULATE`, `&&`)

* **Objective:** Isolate total revenue generated from online credit card sales.
* **DAX:**
Online Credit Card Sales = 
CALCULATE(SUM(Sales[Sales]),Sales[SalesChannel] = "Online" && Sales[PaymentType] = "Credit Card")

---

### Scenario 6: Iterative Counting with Filters (`COUNTX`, `FILTER`)

* **Objective:** Count transaction IDs for high-ticket line items (> 5,000 INR).
* **DAX:**

High Ticket Count = COUNTX(FILTER(Sales, Sales[UnitPrice] > 5000),Sales[TransactionID])

---

### Scenario 7: Exclusion Logic (`CALCULATE`, `<>`)

* **Objective:** Reconcile bank deposits by calculating revenue excluding cash transactions.
* **DAX:**
Non-Cash Sales = CALCULATE(SUM(Sales[Sales]), Sales[PaymentType] <> "Cash")

---

### Scenario 8: Nested `IF` inside an Iterator (`SUMX`)

* **Objective:** Evaluate line-item promo eligibility ($\ge 10$ quantity = 10% discount) row-by-row.
* **DAX:**
Total Promotional Discount = 
SUMX(Sales, IF(Sales[Quantity] >= 10, Sales[Sales] * 0.10, 0))

---

### Scenario 9: Applying Logical OR (`CALCULATE`, `IN`)

* **Objective:** Aggregate transaction counts across "Retail" and "Distributor" sales channels.
* **DAX:**
Retail & Distributor Txns = CALCULATE(COUNT(Sales[TransactionID]), Sales[SalesChannel] IN {"Retail", "Distributor"})

---

### Scenario 10: Combining Measures (Measure Branching)

* **Objective:** Calculate the sales contribution percentage of international markets (e.g., USA).
* **DAX:**

Total Sales = SUM(Sales[Sales])

USA Sales = CALCULATE([Total Sales], Sales[Country] = "USA")

USA Sales % = [USA Sales] / [Total Sales]

---

## 🚀 Key Learning Takeaways & Extensions

* **Iterators vs. Aggregators:** Use `SUMX`/`COUNTX` when calculating row-level dynamic logic before aggregating.
* **Divide-by-Zero Safety:** Upgrade standard division (`/`) to `DIVIDE(Numerator, Denominator, 0)` in production environments to avoid runtime errors.
* **RAM Efficiency:** Prefer Measures over Calculated Columns where dynamic evaluation is required to save memory in the VertiPaq engine.
