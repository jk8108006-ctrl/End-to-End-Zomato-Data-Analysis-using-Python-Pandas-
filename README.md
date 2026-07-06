<div align="center">

# 🍽️ Zomato Bangalore — Restaurant Data Analysis

### End-to-End Data Cleaning, Feature Engineering & EDA on 1,248 Bangalore Restaurants

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Numerical-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557C?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-4C72B0?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

</div>

---

## 📌 Overview

This project takes a **raw, messy Zomato Bangalore restaurant dataset** and runs it through a complete data science pipeline — from cleaning inconsistent real-world data to uncovering business insights through exploratory analysis and visualization.

The goal: answer questions like *"Do restaurants with online ordering get better ratings?"*, *"Which locations dominate the restaurant scene?"*, and *"Does price actually predict quality?"* — using nothing but pandas, statistics, and honest analysis.

---

## 📊 Dataset

| Detail | Value |
|---|---|
| Source file | `zomato_raw.csv` |
| Rows | 1,248 restaurants |
| Raw columns | 11 |
| Final columns (post feature engineering) | 14 |
| Locations covered | 50 |
| Cuisine types | 17 |

**Raw columns:** `name`, `online_order`, `book_table`, `rate`, `votes`, `location`, `rest_type`, `cuisines`, `approx_cost(for two people)`, `listed_in(type)`, `listed_in(city)`

---

## 🧹 1. Data Cleaning

Real-world data is messy — this dataset had null values, inconsistent formatting, and mixed data types across almost every column.

| Column | Issue | Fix |
|---|---|---|
| `book_table` | 122 missing values | Filled with **mode** |
| `rate` | Formats like `4.7/5`, `NEW`, `-`, and 27 nulls | Custom parser function to extract numeric rating → nulls filled with **median (3.7)** |
| `rest_type` | 65 missing values | Filled with `"Unknown"` |
| `cuisines` | 78 missing values | Filled with `"Unknown"` |
| `approx_cost(for two people)` | Mixed types (`"2,500"` as string vs `2500` as int), 96 nulls | Stripped commas → converted to numeric → filled nulls with **median (₹600)**, since distribution was right-skewed (skew = 1.27) |

Columns `listed_in(type)` and `listed_in(city)` were renamed to `Meal_type` and `City` for clarity.

### Outlier Handling (IQR Method)

Applied the IQR method on `approx_cost(for two people)`:

- **Q1:** ₹350 · **Q3:** ₹1000 · **IQR:** ₹650
- **Fences:** Lower = -₹625, Upper = ₹1975
- **150 outlier rows** detected and **capped** (not dropped) using `.clip()`

**Impact of capping:**

| Metric | Before | After |
|---|---|---|
| Max cost | ₹2500 | ₹1975 |
| Mean cost | ₹812 | ₹780 |
| Std deviation | ₹630 | ₹553 |

Capping reduced variance without losing any rows — visualized with side-by-side boxplots and histograms.

---

## ⚙️ 2. Feature Engineering

Three new features were engineered to make the raw data analysis-ready:

- **`price_category`** — Bucketed cost into `Budget` (≤₹350) / `Mid-Range` (≤₹700) / `Premium` (≤₹1200) / `Luxury` (>₹1200)
- **`rating_label`** — Bucketed rating into `Poor` / `Average` / `Good` / `Very Good` / `Excellent`
- **`primary_cuisine`** — Extracted the first cuisine from multi-value strings like `"North Indian, Chinese, Continental"` → `"North Indian"`

---

## 🔎 3. Exploratory Data Analysis

### Key Findings

**a) Online vs Offline Ordering**
| | Avg Rating | Avg Cost | Count |
|---|---|---|---|
| No online order | 3.70 | ₹770.76 | 619 |
| Online order available | 3.67 | ₹789.03 | 629 |

→ Basically no difference. Online ordering doesn't move the rating needle.

**b) Top Locations** — Richmond Town leads with 52 restaurants, followed by Rajajinagar (49) and Hebbal (49).

**c) Price vs Rating** — Mid-Range restaurants had the *highest* average rating (3.71), not Luxury (3.66). Higher price ≠ higher rating.

**d) Correlation Matrix**

| | rate | votes | cost |
|---|---|---|---|
| **rate** | 1.000 | -0.050 | -0.026 |
| **votes** | -0.050 | 1.000 | -0.019 |
| **cost** | -0.026 | -0.019 | 1.000 |

All three variables are essentially **uncorrelated**. The headline insight of this project: *neither cost nor popularity (votes) predicts rating quality* — busting the common assumption that expensive or popular restaurants are automatically rated higher.

**e) Top Cuisines** — South Indian (186), Biryani (140), and North Indian (133) dominate the Bangalore restaurant scene.

---

## 📈 4. Visualizations

A 6-panel dashboard summarizing the full analysis:

![Zomato EDA Dashboard](zomato_eda.png)

1. Rating distribution (with mean/median markers)
2. Top 10 locations by restaurant count
3. Price category breakdown (pie chart)
4. Rating comparison: online vs offline ordering (boxplot)
5. Correlation heatmap
6. Cuisine distribution

---

## 🛠️ Tech Stack

- **Python 3**
- **Pandas** — data cleaning, groupby, pivot tables
- **NumPy** — numerical operations
- **Matplotlib & Seaborn** — visualization



## 🚀 How to Run

```bash
git clone https://github.com/<your-username>/zomato-bangalore-eda.git
cd zomato-bangalore-eda
pip install pandas numpy matplotlib seaborn
jupyter notebook Zomato_Data_Analysis.ipynb
```

---

## 🎯 Key Takeaways

- Cleaned a dataset with nulls across 5+ columns using context-appropriate strategies (mode, median, custom parsing) instead of blindly dropping rows
- Applied IQR-based outlier capping and quantified its effect on distribution
- Engineered 3 new features to enable richer segmentation
- Used groupby, pivot tables, and correlation analysis to test real business assumptions
- Found that popularity and price **do not** predict rating — a counter-intuitive, data-backed insight

---

<div align="center">

**Built as part of a hands-on data science learning journey** 📊

</div>
