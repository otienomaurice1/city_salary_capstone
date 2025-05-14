# Employee Compensation Case Study

## By Biose Ugbo, Maurice Otieno, Yoryi Roque, and Wanos Bahiru

---

### 📘 Introduction

As college students and aspiring data scientists, we are curious about salary variations across different roles and cities. A key decision young professionals face when starting their careers is choosing where to work. To explore this, we focused on New York City, Philadelphia, and San Francisco — major urban centers with extensive government jobs.

Government salary data is more transparent and structured, making it easier to analyze. We collected the data from:
- [San Francisco](https://catalog.data.gov/dataset/employee-compensation)
- [New York](https://catalog.data.gov/dataset/citywide-payroll-data-fiscal-year)
- [Philadelphia](https://catalog.data.gov/dataset/city-employee-earnings)

Each city had different formats, naming conventions, and columns. We cleaned and standardized the data to focus on six key variables:
- `job_title`
- `department`
- `base_salary`
- `overtime_pay`
- `other_pay`
- `city`

After processing, we created a unified dataset of **~2.7 million rows**, saved as `placeholder.csv`.

---

### 📂 Data Dictionary

Each row represents a public employee. The table below shows the structure:

![Data Dictionary](./assets/newplot.png)

---

### 📊 Exploratory Data Analysis (EDA)

#### 🔹 Salary Distribution by City

We began by removing base salaries under $500 to clean anomalies and zero entries. Then we plotted histograms for each city.

**Philadelphia** shows two clear peaks (around $60K and $90K), suggesting two dominant pay bands.  
**New York** has a broader and noisier distribution.  
**San Francisco** is right-skewed with many high earners.

- **Philadelphia Histogram**  
  ![Philly Histogram](./assets/download%20(2).png)

- **New York Histogram**  
  ![NY Histogram](./assets/download%20(3).png)

- **San Francisco Histogram**  
  ![SF Histogram](./assets/download%20(4).png)

---

#### 🔹 Smoothed Salary Distribution

To better visualize trends, we used kernel density estimation (KDE):

![Smoothed KDE Distribution](./assets/download%20(5).png)

- **San Francisco** has the broadest and highest-paid curve.
- **Philadelphia** and **New York** show peaks in the $50K–$100K range.

---

#### 🔹 Boxplot: Salary by City

This box plot confirms that **San Francisco has the highest median salary**, followed by New York, then Philadelphia.

![Boxplot by City](./assets/download%20(6).png)

---

#### 🔹 Total Compensation Breakdown by City

We analyzed how much of each employee’s pay came from:
- Base salary
- Overtime
- Other pay (bonuses, stipends, etc.)

![Compensation Breakdown](./assets/download%20(7).png)

🧠 **Insight**:
- In NYC, “other pay” makes up **25%** of total compensation — much higher than in SF or Philly.
- Overtime is modest across all cities, highest in public safety departments.

---

#### 🔹 Top 10 Departments by Overtime Pay

![Top Overtime Departments](./assets/download%20(8).png)

- The **Sheriff**, **Fire Department**, and **Police** dominate overtime compensation.
- These roles often require 24/7 staffing and have frequent overtime needs.

---

#### 🔹 Top 10 Job Titles by Median Base Salary

![Top Median Salaries](./assets/download%20(9).png)

Roles like **Fire Captain**, **Medical Examiner**, and **Deputy Commissioner** earn the highest median salaries.

---

### 📈 Feature Importance from Random Forest Model

We trained a Random Forest Regressor to predict base salary using city and compensation variables.

![Feature Importance](./assets/download%20(10).png)

🧠 **Key Finding**:  
`other_pay` was the most important feature — even more than `city`.

---

### 🤖 Machine Learning Summary

We trained multiple ML models to predict `base_salary` using:

- `city` (categorical)
- `job_title` (categorical)
- `overtime_pay` (numeric)
- `other_pay` (numeric)

Best model: **LightGBM Regressor**

- **R² Score**: 0.7163 (explains 71.63% of salary variation)
- **MAE**: $25,643.83

🔍 Why is `other_pay` so predictive?

> High-paying jobs often **also** receive stipends, bonuses, and other incentives — even though those aren’t part of base salary.

---

### 📊 Hypothesis Testing

We used the **Kruskal-Wallis test** to confirm:

- **Job title** has the biggest impact on total pay (H = 717,512, p = 0)
- **City** also affects pay, but less so (H = 58,469, p = 0)

✅ This supports our original hypothesis that **compensation is influenced by both city and job classification.**

---

### 📁 Data Access & Notebook

📂 **Google Drive Dataset Folder**  
[Click here to access the data](https://drive.google.com/drive/folders/1HWVh9lLD9AbmIfwc7M2vn55fbR9ZKStM?usp=drive_link)

📓 **Colab Notebook**  
[Open Colab Notebook](https://colab.research.google.com/drive/159OHt7bwRdVXZFmCjwO2d_RVej248dP9)

---

### 🎥 Project Videos

- 🎬 [Tutorial Walkthrough](https://youtu.be/95L-Ps0Roj4)
- 🎥 5-Minute Assertion-Evidence Presentation (link TBD)

---

### 🙌 Acknowledgments

Thanks to:
- Professor Lopez – DS-201
- Open data portals of NYC, SF, and Philadelphia
- Python packages: `pandas`, `seaborn`, `matplotlib`, `lightgbm`, `sklearn`

---

### 💬 Contact

**Biose Ugbo**  
📧 ugbob@lafayette.edu  
🌐 [GitHub Profile](https://github.com/your-username)
