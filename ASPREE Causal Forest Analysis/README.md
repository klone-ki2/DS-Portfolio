#  ASPREE Causal Forest Analysis

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

