
# Enterprise Entry, Survival, and Growth across UK Industries (2019–2023)

This project analyses UK industry-level business demography data (ONS) to understand how **enterprise births** differ across industries and time. It combines cleaned ONS tables into a single master panel, uses **exploratory data analysis (EDA)** to describe patterns and relationships, and then evaluates **forecast-style prediction models** using lagged indicators and time-aware cross-validation.

## Research Questions

**RQ1:** How do enterprise births vary across UK industries between 2019 and 2023, and how are these differences associated with industry size, enterprise survival, and high-growth activity?

**RQ2:** How accurately can enterprise births be predicted using industry-level business demography indicators, and how does the predictive performance of multiple linear regression compare with random forest regression when evaluated using cross-validation on a limited dataset?

## Key Findings (Summary)

* Enterprise births are **highly uneven across industries**: a small number of industries account for a large share of total births, and distributions are strongly **right-skewed**.
* Differences remain even after adjusting for size using **birth intensity** (births per 1,000 active enterprises), suggesting entry patterns are **not explained by scale alone**.
* In bivariate patterns, births tend to be more strongly associated with **industry size** and **high-growth activity** than with short-term survival rates.
* For prediction, a **naive lagged-births baseline** performs strongly on typical errors (MAE), while **random forest** can improve performance for larger errors (RMSE). Overall gains over simple baselines are modest given the short panel and only two test folds.

---

## Repository Structure

Suggested structure (adjust paths if yours differ):

* `Dataset/ONS_Business_Demography/`
  Cleaned CSVs extracted from the ONS Excel workbook (one file per table).
* `Dataset/Final_Master_Datasets/`
  Final merged dataset used in analysis:
  `master_panel_industry_characteristics_2019_2023.csv`
* `Scripts/`
  R scripts (data extraction, merging, EDA, modelling).
* `Outputs/Visuals/`
  Saved figures from EDA and modelling.
* `Outputs/Tables/`
  Saved metrics, predictions, and summary tables.

---

## R Code (What each script does)

### 1) Data extraction (from the ONS Excel workbook)

These scripts read ONS tables and save tidy CSV outputs (long format unless noted):

* `Table_1_2_Births.R` → births of new enterprises by industry/year
* `Table_2_2_Deaths_New.R` → deaths of new enterprises by industry/year
* `Table_3_2_Active.R` → active enterprises by industry/year
* `Table_7_2_HighGrowth.R` → high-growth enterprises by industry/year
* `Table_7_4_Active10Plus.R` → active enterprises (10+ employees) by industry/year
* `Table_5_2_Survival_2019_2023.R` → births + survival (1–5 years) by industry/year (wide)

> Note: Filenames above are examples. Use your actual script names, but keep the order and logic.

### 2) Build master panel (merge all indicators)

* `Build_Master_Panel.R`
  Reads the cleaned CSV files and merges them into:
  `Dataset/Final_Master_Datasets/master_panel_industry_characteristics_2019_2023.csv`

### 3) EDA (RQ1)

* `EDA_RQ1.R`
  Produces distributions, time patterns, industry comparisons, association plots, and correlation matrices to justify feature choices for modelling.

### 4) Modelling (RQ2)

* `RQ2_Modelling.R`
  Builds lagged predictors (t-1), runs time-aware cross-validation, compares:

  * **Naive lag baseline**
  * **Linear regression (lag-only)**
  * **Random forest (lag-only, tuned)**
    Exports metrics, predictions, and diagnostic plots.

---

## How to Download and Run

### Step 0 — Requirements

Install **R** and **RStudio** (recommended).
This project was run using R (version may vary).

### Step 1 — Get the repository

Clone or download this repository to your computer.

### Step 2 — Ensure the dataset is in the correct place

Place the ONS Excel file here:

`Dataset/ONS_Business_Demography/ons_original.xlsx`

(If you rename the file or move it, update the `file_path` inside the scripts.)

### Step 3 — Install required R packages

Run the following in R once:

```r
install.packages(c(
  "tidyverse", "readxl", "readr", "dplyr", "tidyr", "stringr",
  "scales", "corrplot", "tidymodels", "ranger", "rsample", "purrr"
))
```

> Note: `tidyverse` already includes several packages above, but listing them explicitly helps reproducibility.

### Step 4 — Run scripts in order

Run scripts in this order:

1. **Data extraction scripts** (create cleaned CSV files from the Excel workbook)

   * births (Table 1.2)
   * deaths of new enterprises (Table 2.2)
   * active enterprises (Table 3.2)
   * high-growth enterprises (Table 7.2)
   * active enterprises 10+ employees (Table 7.4)
   * survival tables 5.2a–5.2e (2019–2023)

2. **Build the master panel**

   * `Build_Master_Panel.R`

3. **EDA for RQ1**

   * `EDA_RQ1.R`
     Outputs saved to: `Outputs/Visuals/`

4. **Modelling for RQ2**

   * `RQ2_Modelling.R`
     Outputs saved to:
     `Outputs/Tables/` (metrics + predictions) and `Outputs/Visuals/` (plots)

---

## Outputs You Should Expect

After running all scripts, you should see:

* `Dataset/Final_Master_Datasets/master_panel_industry_characteristics_2019_2023.csv`
* Multiple figures in `Outputs/Visuals/` (EDA + modelling diagnostics)
* Model results in `Outputs/Tables/`, including:

  * `RQ2_timeCV_metrics_with_baseline_rmse_r2_mae.csv`
  * `RQ2_timeCV_predictions_log_births_all_models.csv`
  * Random forest tuning tables and summary CSVs

---

## Notes on Reproducibility

* A fixed random seed is used in modelling (`set.seed(123)`).
* Cross-validation is **time-aware** (train years precede test years) to avoid using future information.
* The dataset is short (2019–2023), so results should be interpreted as **indicative**, not definitive.

---
# IJC447-Introduction-to-Data-Science-Coursework-Project
