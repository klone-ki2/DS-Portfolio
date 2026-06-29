# Projects
1. Retail Demand Forecasting & Causal Inference
2. ASPREE Causal Forest Analysis


# 1. Retail Demand Forecasting & Causal Inference

##  Overview
Every inventory decision a retailer makes is a bet on the future. Order too much, and you tie up cash and risk spoilage; order too little, and you miss sales and lose customer trust. The goal of this project is to make that bet less of a gamble using the **Walmart M5 retail dataset**. 

This repository is broken into two core projects covering the two halves of the demand-science job:
1. **Predictive Modeling:** *How much will we sell?* A full forecasting pipeline, from raw data to a machine learning model that produces probabilistic forecasts you can set safety stock against.
2. **Causal Inference:** *If we change something (or an event occurs), how much does demand move because of that specific factor?* Using SNAP food-assistance days as a natural experiment to triangulate causal effects.

## 📊 Dataset
**[M5 Forecasting – Accuracy (Kaggle)](https://www.kaggle.com/competitions/m5-forecasting-accuracy)**
* **Scope:** 5.5 years of daily sales (Jan 29, 2011 – Jun 19, 2016).
* **Scale:** 42,840 hierarchical daily time series across 3 US states (CA, TX, WI), 10 stores, 3 categories (Foods, Hobbies, Household), and 3,049 products.
* *Note: The deep modeling pipeline in this repository was run specifically on the `CA_1` store.*

## 📂 Repository Structure

### Project 1: Demand Forecasting
* **`01_P1_data_prep.ipynb` - Data Preparation & Cleansing**
  * Profiled and memory-optimized raw M5 tables.
  * Reshaped data into a tidy daily panel, aligning calendar, event, SNAP, and price data.
  * Separated structural pre-launch zeros from genuine intermittent zeros and flagged out-of-stock weeks.
  * Applied robust Hampel outlier filters suited for spiky retail demand.

* **`02_P1_eda_timeseries.ipynb` - Deep EDA & Time-Series Diagnostics**
  * Classified demand intermittency (Syntetos–Boylan).
  * Decomposed time series using STL and confirmed differencing orders with ADF/KPSS tests.
  * Engineered leakage-safe features (lags, rolling metrics) via ACF/PACF analysis.
  * Addressed multicollinearity (VIF) and compressed the exogenous calendar block with PCA.

* **`03_P1_modeling.ipynb` - Modeling & Validation**
  * Benchmarked Naive, Seasonal-Naive, ETS, and SARIMA models against global LightGBM/XGBoost models.
  * Implemented strict expanding-window time-series cross-validation.
  * Tuned using Tweedie loss and evaluated with inventory-aware metrics: RMSSE, wRMSSE, MAPE, and Quantile (Pinball) Loss for safety-stock targets.

### Project 2: Supply Chain Causal Inference
* **`04_P2_causal_inference.ipynb` - Causal Analysis of SNAP Days**
  * Analyzed SNAP (Supplemental Nutrition Assistance Program) days as a natural experiment.
  * Demonstrated how naive comparisons are confounded by seasonality/promotions.
  * Triangulated the true causal effect using three complementary methods:
    1. **Placebo Control:** Checking non-food categories (`HOBBIES`) for false positives.
    2. **Difference-in-Differences (DiD):** Measuring the `FOODS` effect net of calendar shocks.
    3. **Propensity-Score / Inverse Probability Weighting (IPW):** Quantifying confounding bias.

## 🚀 Key Results

### 1. Forecasting Accuracy (RMSSE - lower is better, <1 beats seasonal-naive)
* **LightGBM:** `0.595` (Winner)
* **Exponential Smoothing (ETS):** `0.636`
* **SARIMA:** `0.649`
* **Seasonal-Naive (Baseline):** `0.681`
* **Naive:** `2.676`
* **Cross-validated RMSSE (expanding window):** `0.566 ± 0.094`
* **Probabilistic Coverage:** The 95th-percentile model achieved **94.5% empirical coverage**, meaning it kept stock sufficient ~19 out of 20 days.

### 2. Causal Effect of SNAP Days on Food Demand
* **Naive (Biased) Estimate:** `+17.3%`
* **Difference-in-Differences:** `+15.1%` (95% CI: 14.2% - 16.0%)
* **Propensity Score (IPW):** `+14.8%` 
* *Takeaway:* The naive method overstates the lift due to confounding variables. Planning inventory purely on the naive estimate leads to overstocking.

## 🛠️ Tools & Libraries
* **Python:** `pandas`, `numpy`, `scikit-learn`, `statsmodels`, `scipy`
* **Modeling:** `LightGBM`, `XGBoost`
* **Causal Inference:** Difference-in-Differences, Inverse Probability Weighting (IPW)
----------
----------
---------
---------
# 2 ASPREE Causal Forest Analysis

## Overview
This repository contains the technical documentation and methodology for a **Causal Forest Analysis** conducted on the **ASPREE U.S. cohort**. 

The study moves beyond traditional "Average Treatment Effect" (ATE) analysis by utilizing non-parametric machine learning to identify **Heterogeneous Treatment Effects (HTE)**—helping to transition toward precision geriatric medicine by pinpointing specific subgroups that derive clear clinical benefits from aspirin therapy.

## Table of Contents
1. [Problem Statement](#1-problem-statement)
2. [Methodology](#2-methodology)
3. [Causal Inference Pipeline](#3-causal-inference-pipeline)
4. [Key Findings](#4-key-findings)
5. [Conclusion](#5-conclusion)

---


## 1. Problem Statement
Traditional randomized controlled trial (RCT) analysis often assumes treatment homogeneity. However, clinical interventions often vary in their impact. This study aims to estimate **Conditional Average Treatment Effect (CATE)** to identify which individuals in the ASPREE cohort benefit most from aspirin versus placebo.

## 2. Methodology
The analysis employs a **Causal Forest**, a machine learning approach optimized for causal inference.
* **Objective:** Maximize variance in treatment effect estimates rather than minimizing prediction error.
* **Data:** 28 baseline clinical metrics (physical and cognitive).
* **Tuning:** * "Honest Splitting" to ensure valid inference.
    * 100 trees with 10-node pruning to prevent overfitting.
    * Stratified sampling to manage class imbalance.

## 3. Causal Inference Pipeline
The study followed a rigorous workflow:
1. **Data Partitioning:** Stratified splitting of the cohort (n=1,141 training/validation, n=1,150 testing).
2. **Estimation:** Generation of individual CATE scores.
3. **Validation:** Use of Wald Chi-Squared tests to confirm the statistical significance of identified heterogeneity.

## 4. Key Findings
* **Performance:** Achieved an **AUC of 0.74**, validating the model's ability to rank patient responsiveness.
* **Clinical Impact:** Participants in the highest-risk quintile achieved a **13.7% Absolute Risk Reduction (ARR)** (95% CI 3.1% to 24.4%) in primary composite outcomes after 5 years of aspirin treatment compared to the placebo group.

## 5. Conclusion
By identifying a high-risk subgroup where aspirin provided clear benefit, this research demonstrates the power of machine learning in uncovering hidden, clinically significant treatment effects within medium-sized clinical trial cohorts.
