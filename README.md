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

After aligning and cleaning formats, our unified dataset contained the following columns:

| Column         | Description |
|----------------|-------------|
| `job_title`    | Job role/classification |
| `department`   | Government agency or department |
| `base_salary`  | Fixed annual compensation |
| `overtime_pay` | Pay for hours worked beyond normal schedule |
| `other_pay`    | Bonuses, reimbursements, stipends, etc. |
| `city`         | Philadelphia, New York, or San Francisco |

Final dataset size: **~2.7 million rows**

---

## 📈 Exploratory Data Analysis (EDA)

Using boxplots, bar charts, and medians, we explored salary distributions across:

- Job titles
- Departments
- Cities

### Key Insights:
- **Firefighters**, medical staff, and department heads consistently rank among the highest-paid employees.
- **San Francisco** shows the highest median base salary overall.
- `other_pay` and `overtime_pay` vary greatly by city and strongly impact total compensation.

---

## 🤖 Machine Learning: LightGBM Regressor

We trained a **LightGBM regression model** to predict `base_salary` based on:

- `job_title`
- `city`
- `overtime_pay`
- `other_pay`

To reduce skew, we applied a **log transformation** to the salary target and inverse-transformed predictions for evaluation.

### ⚙️ Model Details:
- Algorithm: LightGBM Regressor
- Features: Encoded job title and city + scaled overtime and other pay
- Sample size: 8,000 records for performance

### 📊 Results:
- **R² Score**: 0.1252
- **MAE (Mean Absolute Error)**: $25,643.83

---

## 🧠 Interpreting Results

Although city names were included, the model found that **extra compensation components** (especially `other_pay`) were the most powerful predictors of salary.

> *This suggests that job type and compensation structure (bonuses, hazard pay, retirement payouts) are more important than geography alone when it comes to explaining base salary differences.*

---

## 🎥 Project Videos

📹 **Colab Tutorial Walkthrough**  
Watch how we prepared the data and trained the model  
➡️ [YouTube Link – Tutorial](https://your-tutorial-link.com)

📹 **5-Minute Assertion-Evidence Presentation**  
Summary of questions, insights, and takeaways  
➡️ [YouTube Link – Presentation](https://your-presentation-link.com)

---

## 📁 Repository Structure




[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/159OHt7bwRdVXZFmCjwO2d_RVej248dP9)
Slides: https://docs.google.com/presentation/d/1hezgQOHoLfX9G5gEsrmQ3CcrQoJRH5h65-TzNkej9No/edit?usp=sharing


---

## 🚀 How to Reproduce

1. Clone the repo or open the notebook in Google Colab  
2. Upload `placeholder.csv` if not already included  
3. Run each notebook cell in order  
4. Outputs will include charts, predictions, and final evaluation metrics

---

## 📌 Key Takeaways

- High-paying public sector roles are tightly tied to job type, not just city.
- `other_pay` (bonuses, stipends, reimbursements) is a major signal for seniority or specialization.
- LightGBM performed reasonably well given the lack of deeper features like experience level or job grade.
- Geographic effects are visible but largely explained by differences in compensation structure across cities.

---

## 🙌 Acknowledgments

Thanks to:
- DS-201 instructors and TAs  
- City governments of New York, San Francisco, and Philadelphia for providing transparent public data  
- Open-source Python libraries used: Pandas, Scikit-learn, LightGBM, Seaborn, Matplotlib

---

## 📬 Contact

For questions or collaboration:  
**Biose Ugbo** – ugbob@lafayette.edu



