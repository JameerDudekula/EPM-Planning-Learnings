# EOSB Monthly Accrual Calculation Rule

## 📌 Overview

This business rule calculates End of Service Benefit (EOSB) on a **monthly basis**, replacing the earlier approach where EOSB was calculated as a one-time yearly value.

---

## 🎯 Business Requirement

* Earlier: EOSB was calculated only once (year-end)
* New Requirement:

  * Calculate EOSB **every month**
  * Reflect **progressive accrual** over time
  * Handle employee-specific conditions like tenure, category, and grade

---

## 🔄 Key Enhancement

* Converted EOSB logic from **single-period calculation → multi-period (Jan–Dec) calculation**
* Introduced **monthly accrual logic**
* Ensured aggregation at higher dimensions after calculation

---

## 🧠 Logic Explanation

### 1. Data Reset

* Clears existing EOSB values for the calculation scope before recalculating

### 2. Tenure Calculation

* Uses hire date and plan period end date
* Handles edge case for employees completing anniversary during plan year

### 3. Monthly Processing

* Executes calculation across all months (Jan–Dec)
* EOSB accrues progressively instead of a single snapshot

### 4. Conditional Logic

EOSB varies based on:

* Employee category (genericized)
* Grade group
* Service tenure slabs:

  * 1–5 years
  * 5–10 years
  * 10+ years

### 5. Accrual Calculation

* Monthly salary derived from annual salary
* EOSB calculated using tenure-based multipliers
* Two outputs:

  * Total EOSB Accrual
  * Plan Year Contribution

### 6. Aggregation

* Post-calculation aggregation across:

  * Cost Center
  * Employee
  * Category
  * Grade

---

## ⚙️ Key Technical Highlights

* Used `FIX` to control calculation scope
* Applied `@CalcMgrDateDiff` for tenure calculation
* Implemented nested `IF-ELSE` for slab-based logic
* Used multi-dimensional checks (`@ISMBR`) for segmentation
* Ensured clean recalculation using `CLEARBLOCK`
* Aggregated results using `AGG`

---

## ⚠️ Important Considerations

* Logic is genericized to avoid sensitive business exposure
* Salary and employee identifiers are abstracted
* Edge cases like hire date alignment handled separately
* Performance considered by restricting FIX scope

---

## 🧪 Testing Approach

* Tested across:

  * Different tenure ranges
  * Multiple employee categories
  * Edge cases (new joiners, anniversary boundary)

* Validated:

  * Monthly accrual consistency
  * Year-end totals match expected values

---

## 🚀 Outcome

* Enabled accurate **monthly financial tracking**
* Improved **forecasting and planning visibility**
* Reduced dependency on manual adjustments
* Scalable logic for future enhancements

---

## 💡 Learnings

* Handling time-based calculations in Planning
* Designing scalable multi-period logic
* Managing complex conditional branching
* Balancing performance with detailed calculations

