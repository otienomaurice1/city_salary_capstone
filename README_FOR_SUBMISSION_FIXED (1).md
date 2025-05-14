# 💼 Employee Compensation Case Study

## By Biose Ugbo, Maurice Otieno, Yoryi Roque, and Wanos Bahiru
**DS-201 | Spring 2025 | Lafayette College**

---

## 📘 Background & Motivation

Choosing where to work is a major decision for young professionals, especially in public service. Our group was curious about how government compensation varies across U.S. cities, and whether job titles or location matter more.

We focused on three cities: **Philadelphia**, **New York City**, and **San Francisco** — all with large public workforces and accessible salary data. Our hypothesis: **employee compensation is influenced by both city and job classification**.

---

## 🗃️ Data Collection & Cleaning

**Source Datasets:**
- [Philadelphia](https://catalog.data.gov/dataset/city-employee-earnings)
- [New York City](https://catalog.data.gov/dataset/citywide-payroll-data-fiscal-year)
- [San Francisco](https://catalog.data.gov/dataset/employee-compensation)

**Cleaning Steps:**
- Selected key columns: `job_title`, `department`, `base_salary`, `overtime_pay`, `other_pay`, `city`
- Standardized column names and salary types across cities
- Removed invalid or missing salary rows
- Final dataset size: ~2.7 million rows

**Data Access:** [Google Drive Folder](https://drive.google.com/drive/folders/1HWVh9lLD9AbmIfwc7M2vn55fbR9ZKStM?usp=drive_link)
---

## 📊 Exploratory Data Analysis (EDA)

### Salary Distribution by City (Histograms)
![Philly](./assets/download%20(2).png)
Philadelphia shows two dominant salary bands around $60K and $90K.

![NY](./assets/download%20(3).png)
New York is noisier with wide dispersion.

![SF](./assets/download%20(4).png)
San Francisco has a long right-skewed tail, with many high earners.

### Smoothed Salary Distribution (KDE)
![KDE](./assets/download%20(5).png)
Philadelphia and NYC exhibit bimodal distributions. SF has a broader, higher-salaried curve.

### Salary Boxplot by City
![Boxplot](./assets/download%20(6).png)
San Francisco has the highest median salary. NYC has a wider range. Philadelphia is more compressed.

### Compensation Structure by City
![Comp Breakdown](./assets/download%20(7).png)
Base salary dominates in all cities. NYC relies heavily on “other pay” (nearly 25%).

### Top Overtime Departments
![OT Depts](./assets/download%20(8).png)
Sheriff, Fire, and Police departments top overtime payouts.

### Top Median Salaries by Job Title
![Top Salaries](./assets/download%20(9).png)
Medical Examiners, Captains, and other specialized roles earn highest base salaries.

### Feature Importance from ML Model
![Feature Importance](./assets/download%20(10).png)
`other_pay` was the strongest predictor of base salary — often tied to seniority and job type.

---

## 🤖 Modeling

**Algorithm Used:** LightGBM Regressor
- Features: `city`, `job_title`, `overtime_pay`, `other_pay`
- Target: `base_salary` (log-transformed)

**Performance Metrics:**
- R^2 = 0.7163 (71.6% variance explained)
- MAE ≈ $25,600

**Takeaways:**
- `job_title` and `other_pay` were far more predictive than `city`
- Suggests internal factors like seniority and role matter more than geography

---

## ✅ Conclusion

- **Job classification** is the most important driver of public sector pay
- **San Francisco** leads in median salary due to cost of living and role specialization
- **NYC's use of other pay** raises questions about equity and transparency
- Machine learning offers useful insight but more detailed HR data (e.g., experience, step levels) would improve predictions

---

## 🔗 Links

- 📓 [Colab Notebook](https://colab.research.google.com/drive/159OHt7bwRdVXZFmCjwO2d_RVej248dP9)
- 📂 [Google Drive Data Folder](https://drive.google.com/drive/folders/1HWVh9lLD9AbmIfwc7M2vn55fbR9ZKStM?usp=drive_link)
- 🎬 [Tutorial Video](https://youtu.be/95L-Ps0Roj4)
- 🧠 Presentation Video: _link coming soon_
