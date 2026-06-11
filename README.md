# Project 1: E-Commerce Order Data Cleaning & Preparation

**Role:** Data Analyst (Industrial Training Kit, DecodeLabs — Batch 2026)
**Tools:** Excel, Python (pandas)
**Dataset:** 1,200 e-commerce order records, 14 columns

---

## 📌 Project Overview

The goal of this project was to take a raw, unprocessed e-commerce orders dataset and transform it into an analysis-ready, "gold standard" dataset — without losing data through careless deletion, and with full documentation of every change made.

The project followed a structured three-phase methodology:

1. **Strategic Imputation** — handle missing values without resorting to listwise deletion
2. **Integrity Audit** — confirm there are zero duplicate records and zero duplicate identifiers
3. **Standardisation** — bring dates, numbers, and text into one consistent format

A full **Change Log** was maintained throughout, recording what was changed, why, the method used, and how many records were affected — following the principle that *"if it isn't documented, it didn't happen."*

---

## 🔍 Data Quality Findings

| Check | Result |
|---|---|
| Total records | 1,200 |
| Total columns | 14 |
| Duplicate rows | 0 |
| Duplicate Order IDs | 0 |
| Negative quantities / prices | 0 |
| Missing values | 309 (CouponCode only) |
| Date format inconsistencies | 1,200 (non-ISO format) |
| Floating-point precision errors | 29 (TotalPrice column) |
| TotalPrice = Quantity × UnitPrice | Verified true for all 1,200 rows |

---

## 🧹 Cleaning Actions Taken

### 1. Missing Values — `CouponCode`
309 records (≈26% of the dataset) had no coupon code. Rather than dropping these rows — which would have reduced statistical power and removed otherwise valid order data — missing values were imputed with the flag **`"NONE"`**, clearly indicating "no coupon applied" as a distinct, analyzable category.

### 2. Duplicate Audit — `OrderID`
Ran a full duplicate check across all rows and specifically on `OrderID`. Result: **0 duplicate rows, 0 duplicate Order IDs**. All 1,200 identifiers are unique — the dataset passed this integrity gate with no corrective action needed.

### 3. Date Standardisation — `Date`
All order dates were converted to **ISO 8601 format (YYYY-MM-DD)** for consistency and to support reliable sorting, filtering, and time-series analysis.

### 4. Numeric Precision — `UnitPrice` & `TotalPrice`
29 `TotalPrice` values contained floating-point artefacts (e.g. `769.3799999999999`) caused by repeating-decimal multiplication. All monetary values were rounded to **2 decimal places** using standard financial rounding.

### 5. Text Formatting — All String Columns
- Stripped leading/trailing whitespace from all 9 text columns (`OrderID`, `CustomerID`, `Product`, `ShippingAddress`, `PaymentMethod`, `OrderStatus`, `TrackingNumber`, `CouponCode`, `ReferralSource`)
- Standardised `Product` and `OrderStatus` to **Title Case** for consistent categorical values

### 6. Cross-Column Validation
Verified that `TotalPrice = Quantity × UnitPrice` held true (within $0.01 tolerance) across all 1,200 records. **0 mismatches found** — confirming the dataset's internal mathematical consistency.

---

## ✅ Verification Gate Results

| Metric | Requirement | Result | Status |
|---|---|---|---|
| Duplicate Order ID rate | 0% | 0% (1,200 unique IDs) | ✅ PASS |
| Incorrect date format rate | 0% | 0% (all YYYY-MM-DD) | ✅ PASS |
| Missing values handled | 100% | 100% (CouponCode imputed) | ✅ PASS |
| Numeric precision | 2 decimal places | 2 d.p. (29 values corrected) | ✅ PASS |

---

## 📂 Deliverables

- **[DecodeLabs_Project1_Cleaned_Dataset.xlsx](./DecodeLabs_Project1_Cleaned_Dataset.xlsx)** — Excel workbook containing:
  - `Cleaned Data` — final analysis-ready dataset (1,200 rows)
  - `Raw Data` — original dataset for audit comparison
  - `Change Log` — detailed record of all 7 cleaning actions (CR001–CR007), with verification gate summary
  - `Summary Stats` — post-cleaning dataset overview (product mix, order status, payment methods, revenue metrics)

---

## 💡 Key Takeaways

- **Don't delete, investigate first.** A 26% "missing data" rate looked alarming at first glance, but turned out to be a single column where blanks carried meaningful information (no coupon used).
- **Floating-point errors are silent.** Numbers that *display* fine in a spreadsheet can carry hidden precision artefacts that break exact-match comparisons — always round monetary fields explicitly.
- **Documentation is part of the deliverable.** A change log transforms "I cleaned the data" into a reproducible, auditable process that stakeholders (and future analysts) can trust.
