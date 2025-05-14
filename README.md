

# 💼 Public Paycheck: Predicting Government Salaries Across U.S. Cities

## 👥 Team Members
- Biose Ugbo  
- Maurice Otieno  
- Yoryi Roque  
- Wanos Bahiru

## 📘 Course: DS-201 – Final Project  
Spring 2025 | Lafayette College

---

## 🧠 Project Overview

This project explores government salary data from **New York City**, **Philadelphia**, and **San Francisco** to answer key questions about how compensation varies across cities and job roles.

We cleaned and merged over 2.7 million public employee records, standardized job and salary components, and built machine learning models to understand the most influential factors driving salary differences.

---

## 🎯 Research Questions

1. **What job classifications are associated with higher salaries within government programs?**
2. **Is there a difference in employee compensation for comparable positions across major cities?**

---

## 📊 Data Summary

We used public datasets from:
- [Philadelphia](https://catalog.data.gov/dataset/city-employee-earnings)
- [New York City](https://catalog.data.gov/dataset/citywide-payroll-data-fiscal-year)
- [San Francisco](https://catalog.data.gov/dataset/employee-compensation)

| Column         | Description |
|----------------|-------------|
| `job_title`    | Job classification/title |
| `department`   | Government agency or department |
| `base_salary`  | Base yearly salary (gross) |
| `overtime_pay` | Earnings from overtime work |
| `other_pay`    | Other compensation (e.g., bonuses) |
| `city`         | City the employee works in |

📈 Final dataset size: ~2.7 million records

![Data Dictionary](./assets/newplot.png)

---

## 📈 Visual Insights from EDA

### 🔹 Feature Importance (LightGBM)

![Feature Importance](./assets/download%20(10).png)

This bar chart ranks which features mattered most in predicting base salary. `other_pay` dominated, suggesting that one-time bonuses, stipends, and reimbursements are strong signals of high-salary roles. `overtime_pay` and `city_New York` also had moderate importance.

---

### 🔹 Top 10 Highest Median Salary Job Titles

![Top Median Salaries](./assets/download%20(9).png)

This bar plot shows which job titles have the highest median base salaries. Unsurprisingly, high-level administrators, examiners, and investment advisors top the list, with salaries above $350,000–$450,000.

---

### 🔹 Top 10 Departments by Average Overtime Pay

![Top OT Departments](./assets/download%20(8).png)

Departments like the **Sheriff**, **Police**, and **Emergency Management** lead in average overtime pay — reflecting the nature of round-the-clock service, emergencies, and public safety roles.

---

### 🔹 Compensation Breakdown by City

![Comp Breakdown](./assets/download%20(7).png)

This stacked bar chart breaks down total compensation by city. New York has a higher proportion of `other_pay`, while Philadelphia and San Francisco lean more heavily on base salary. This reinforces that **compensation structure, not just city, affects total earnings.**

---

### 🔹 Base Salary Distribution by City (Box Plot)

![Boxplot by City](./assets/download%20(6).png)

San Francisco has the highest median base salary and the widest spread. Philadelphia’s salaries are lower overall, and NYC has many outliers on both ends. This gives us a snapshot of city-level variation.

---

### 🔹 Smoothed Salary Distributions (Density Plot)

![Smoothed Salary Density](./assets/download%20(5).png)

This kernel density plot smooths salary distribution across cities. San Francisco’s curve skews toward higher earnings, while Philadelphia is tightly clustered around ~$60K–$100K. NYC shows a mixed spread with multiple salary "modes."

---

### 🔹 City-Specific Histograms

**San Francisco**  
![SF Histogram](./assets/download%20(4).png)

**New York**  
![NY Histogram](./assets/download%20(3).png)

**Philadelphia**  
![Philly Histogram](./assets/download%20(2).png)

These histograms reveal how salary distributions differ by city. San Francisco has a long tail of high earners, NYC is more evenly spread, and Philadelphia shows sharp peaks, suggesting standardized salary bands or job groupings.

---

## 🤖 Machine Learning Model

We trained a **LightGBM regression model** to predict base salary using:
- `job_title`
- `city`
- `overtime_pay`
- `other_pay`

### 📈 Results:
- **R² Score**: 0.1252
- **MAE**: $25,643.83

This means the model explains ~12.5% of salary variance and is off by ~$25K on average — reasonable given missing features like seniority or years of service.

---

## 🧠 Final Takeaways

- High-paying roles are closely tied to **job classification**, not just geography.
- **Other pay** (bonuses, stipends, etc.) is a key marker of high earnings.
- NYC had a more complex compensation structure, while SF offered higher base salaries overall.
- Our model performed well given the limitations of public data — many salary decisions are driven by internal HR policies, union contracts, and tenure.

---

## 🎥 Project Videos

📹 **Colab Tutorial Walkthrough**  
[Watch on YouTube](https://youtu.be/mPa75j7P100)

📹 **5-Minute Presentation (Assertion-Evidence)**  
[Watch on YouTube](https://youtu.be/95L-Ps0Roj4)
---

## 📁 Repository Structure
/assets/ <- Graphs and visual outputs
/data/placeholder.csv <- Final dataset (hosted externally if too large)
/notebooks/Final_Project_Code.ipynb <- Full analysis notebook
README.md <- This file


[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/159OHt7bwRdVXZFmCjwO2d_RVej248dP9)  
📊 [Slides](https://docs.google.com/presentation/d/1hezgQOHoLfX9G5gEsrmQ3CcrQoJRH5h65-TzNkej9No/edit?usp=sharing)

---

## 🙌 Acknowledgments

Special thanks to:
- Professor Lopez – DS-201, Spring 2025
- City open data portals of NYC, SF, and Philadelphia
- Open-source libraries: Pandas, Matplotlib, Seaborn, Scikit-learn, LightGBM

---

## 📬 Contact

**Biose Ugbo**  
📧 ugbob@lafayette.edu  
🌐 [GitHub](https://github.com/your-username)


