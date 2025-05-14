
# 💼 Public Paycheck: Predicting Government Salaries Across U.S. Cities

## By Biose Ugbo, Maurice Otieno, Yoryi Roque, and Wanos Bahiru  
DS-201 | Spring 2025 | Lafayette College

---

## 📚 Table of Contents
- [Introduction](#introduction)
- [Data Organization](#data-organization)
- [Exploratory Data Analysis](#eda)
  - [City Salary Distributions](#eda-histograms)
  - [Smoothed Distributions](#eda-kde)
  - [Boxplot: Salary by City](#eda-boxplot)
  - [Compensation Breakdown](#eda-stacked)
  - [Overtime by Department](#eda-overtime)
  - [Top Median Salaries](#eda-top-jobs)
- [Machine Learning](#machine-learning)
- [Conclusion](#conclusion)
- [Links](#links)

---

<a name="introduction"></a>
## 📘 Introduction

As students and aspiring data scientists, we were curious about salary variation in government roles across U.S. cities. We focused on **New York City**, **Philadelphia**, and **San Francisco**, leveraging their public transparency and scale to explore:

> 🔍 *What drives compensation in government? Is it city? Job? Extra pay?*

Our hypotheses:
- H₀: Average salary is the same across cities
- H₀: Job titles do not significantly impact compensation

---

<a name="data-organization"></a>
## 🗃️ Data Organization

We gathered public employee salary data from:
- NYC, SF, and Philadelphia (Data.gov links)
- Extracted: `job_title`, `department`, `base_salary`, `overtime_pay`, `other_pay`
- Added: `city` column
- Cleaned: removed nulls, standardized formats

**Final CSV:** `placeholder.csv`  
**Row count:** ~2.7 million  
**Columns:** 6 (see below)

![Data Dictionary](./assets/e9a7c552-f167-4c78-b6bf-81e7db893db1.png)

---

<a name="eda"></a>
## 📊 Exploratory Data Analysis (EDA)

<a name="eda-histograms"></a>
### 1. Salary Distribution by City

**Philadelphia**
![Philly Hist](./assets/download%20(2).png)

Bimodal distribution, peaks at ~$60K and ~$90K.

**New York**
![NY Hist](./assets/download%20(3).png)

Noisy structure, many entries below $20K. Suggests wide variation or data issues.

**San Francisco**
![SF Hist](./assets/download%20(4).png)

Smooth, right-skewed, large tail with many high earners.

---

<a name="eda-kde"></a>
### 2. Smoothed Salary Distribution (KDE)

![KDE](./assets/download%20(5).png)

SF shows one dominant peak near $75K. Philly and NYC show bimodal behavior.

---

<a name="eda-boxplot"></a>
### 3. Base Salary Boxplot by City

![Boxplot](./assets/download%20(6).png)

- SF has highest median salaries
- NYC is wide and noisy
- Philly is most compressed

---

<a name="eda-stacked"></a>
### 4. Average Compensation Breakdown (Stacked Bars)

![Stacked Comp](./assets/download%20(7).png)

- Base salary makes up ~80–90% across all cities
- NYC shows high reliance on **other_pay** (~25%)

---

<a name="eda-overtime"></a>
### 5. Departments with Highest Average Overtime

![OT Depts](./assets/download%20(8).png)

Sheriff, Police, Fire, and Emergency Management dominate overtime payouts.

---

<a name="eda-top-jobs"></a>
### 6. Top 10 Job Titles by Median Base Salary

![Top Jobs](./assets/download%20(9).png)

- Medical examiners and agency directors are top earners
- Most top jobs are specialized healthcare or executive roles

---

<a name="machine-learning"></a>
## 🤖 Machine Learning

We trained a **LightGBM Regressor** on 5,000 sampled rows with:
- Categorical: `city`, `job_title`
- Numeric: `overtime_pay`, `other_pay`
- Target: `log(base_salary)`

### 📊 Feature Importances
![Feature Importance](./assets/download%20(10).png)

- `other_pay` = #1 predictor of base_salary
- Followed by `overtime_pay` and job roles like firefighter

### 📈 Model Performance
- R² = **0.7163**
- MAE ≈ **$25,643**

**Why other_pay matters:**  
It often appears in high-ranking jobs (e.g. fire chiefs, medical directors). While not causal, it strongly correlates with seniority.

---

<a name="conclusion"></a>
## ✅ Conclusion

- **Job title** is the strongest predictor of salary.
- **City matters**, but mostly via structure of “other” compensation.
- **San Francisco** pays more on average.
- **NYC** has the most irregularities (many $0 base salaries offset by other pay).

---

<a name="links"></a>
## 🔗 Links

- 📓 [Final Notebook in Google Colab](https://colab.research.google.com/drive/159OHt7bwRdVXZFmCjwO2d_RVej248dP9)
- 📂 [Google Drive Folder](https://drive.google.com/drive/folders/1HWVh9lLD9AbmIfwc7M2vn55fbR9ZKStM?usp=drive_link)
- 🎥 [5-Min Presentation](https://youtu.be/95L-Ps0Roj4)
