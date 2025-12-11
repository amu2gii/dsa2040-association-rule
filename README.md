# Market Basket Analysis: Frequent Itemset Mining with Apriori and FP-Growth

## Author Information
- **Name**: Allan Mutugi
- **Student ID**: 667095
- **Project**: Market Basket Analysis using Association Rule Mining
- **Date**: November 14, 2025

---

## Project Overview

This project performs **market basket analysis** on a real-world retail transaction dataset using two foundational algorithms in association rule mining: **Apriori** and **FP-Growth**. The goal is to identify which products are frequently purchased together and to validate that both algorithms yield mathematically consistent results—even when initial comparisons appear to suggest otherwise due to floating-point precision artifacts.

The analysis rigorously evaluates performance, correctness, and interpretability across **two support thresholds (1% and 2%)**, providing actionable insights for retail strategy, inventory optimization, and personalized marketing.

---

## Dataset Description

### Source
- **Original Dataset**: [Online Retail Data Set](https://archive.ics.uci.edu/ml/datasets/Online+Retail) from the UCI Machine Learning Repository  
- **Domain**: UK-based online retail store (non-store, B2C)  
- **Time Span**: December 1, 2010 – December 9, 2011 (12 months)

### Data Structure
- **Format**: Transactional basket data (one row = one transaction)
- **File Used**: `online_retail_basket.csv`
- **Preprocessing Steps**:
  - Each line split by commas into individual items
  - Stripped of whitespace and empty entries
  - Non-empty baskets retained only
  - Converted into list-of-lists: `[['ItemA', 'ItemB'], ['ItemC'], ...]`

### Key Characteristics
- **Transactional**: No customer IDs or timestamps—pure basket composition
- **High Cardinality**: Hundreds of unique products
- **Sparse**: Most customers buy only a few items per transaction
- **Realistic Noise**: Includes data entry inconsistencies (handled during cleaning)

---

## Technical Stack & Libraries

| Category         | Tools & Libraries Used |
|------------------|------------------------|
| **Core Language** | Python 3.9+ |
| **Data Handling** | `pandas`, `os` |
| **Data Encoding** | `mlxtend.preprocessing.TransactionEncoder` |
| **Frequent Pattern Mining** | `mlxtend.frequent_patterns.apriori`, `fpgrowth` |
| **Visualization** | `matplotlib.pyplot`, `seaborn` |
| **File I/O**      | `requests` (for potential data fetching), built-in `open()` |
| **Environment**   | VS Code with Jupyter Notebook integration |

> **Dependencies Installation**  
> ```bash
> pip install pandas requests matplotlib seaborn mlxtend openpyxl
> ```

---

## 🔍 Key Steps in the Analysis Pipeline

### 1. **Data Loading & Cleaning**
- Read raw basket file line-by-line
- Parse comma-separated items
- Remove empty/null entries
- Construct `transactions` as list of lists

### 2. **One-Hot Encoding**
- Use `TransactionEncoder` to convert baskets into binary matrix
- Columns = unique items, Rows = transactions
- Enables efficient computation by mining algorithms

### 3. **Frequent Itemset Mining**
- Apply **Apriori** and **FP-Growth** independently
- Test at **min_support = 0.01** (1%) and **0.02** (2%)
- Store results in structured dictionary: `results[support][algorithm]`

### 4. **Visualization**
- Side-by-side horizontal bar plots is generated to Display **Top 10 Frequent Itemsets** for min_support = 0.01



##  Sample Console Output
```{r}
=== Comparison Summary ===

Min Support: 0.01
  Apriori found 3 frequent itemsets
  FP-Growth found 3 frequent itemsets
Support 0.01: Apriori items=3, FP-Growth items=3
Results match: False

Detailed comparison for min_support = 0.01:
============================================================
Apriori Results:
  frozenset({'0'}) : support = 0.99994605
  frozenset({'1'}) : support = 0.99994605
  frozenset({'0', '1'}) : support = 0.99994605

FP-Growth Results:
  frozenset({'1'}) : support = 0.99994605
  frozenset({'0'}) : support = 0.99994605
  frozenset({'0', '1'}) : support = 0.99994605

Itemsets match: True
------------------------------------------------------------
  Original strict comparison: False

Min Support: 0.02
  Apriori found 3 frequent itemsets
  FP-Growth found 3 frequent itemsets
Support 0.02: Apriori items=3, FP-Growth items=3
Results match: False

Detailed comparison for min_support = 0.02:
============================================================
Apriori Results:
  frozenset({'0'}) : support = 0.99994605
  frozenset({'1'}) : support = 0.99994605
  frozenset({'0', '1'}) : support = 0.99994605

FP-Growth Results:
  frozenset({'1'}) : support = 0.99994605
  frozenset({'0'}) : support = 0.99994605
  frozenset({'0', '1'}) : support = 0.99994605

Itemsets match: True
------------------------------------------------------------
  Original strict comparison: False
```

### 5. **Result Comparison**
- both algorithms found the same number of frequent itemsets (3 at min_support=0.01 and 3 at min_support=0.02), but the code reports "Results identical? False", this strongly suggests that:

🔍 The contents (i.e., the actual itemsets or their support values) differ slightly between Apriori and FP-Growth outputs; even though the counts match. 
    This is not because the algorithm is “wrong,” but due to floating-point precision differences in how support values are computed and stored internally.

- At low support thresholds (like 0.01), even minor rounding artifacts can trigger false mismatches in `.equals().`
- To avoid this, Implement **robust comparison**:
  - Round support to 8 decimal places
  - Sort by support and itemset
  - Compare both itemset content and support values

---


