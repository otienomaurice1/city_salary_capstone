
# Employee Compensation Case Study

## By Biose Ugbo, Maurice Otieno, Yoryi Roque, and Wanos Bahiru

---

## Introduction

As college students and aspiring data scientists, we are curious about salary variations across different roles and cities. A key decision young professionals face when starting their careers is choosing where to work. To explore this, we focused on New York City, Philadelphia, and San Francisco, major urban centers with extensive government jobs that offer diverse employment opportunities. These cities are attractive destinations for young professionals and provide a rich dataset of public sector salaries.

Government jobs are typically stable and offer a wide range of positions. Their compensation data is also more transparent and standardized compared to private sector roles, which makes it easier to analyze salary distributions. We collected the data from [San Francisco](https://catalog.data.gov/dataset/employee-compensation), [New York](https://catalog.data.gov/dataset/citywide-payroll-data-fiscal-year), and [Philadelphia](https://catalog.data.gov/dataset/city-employee-earnings), which house publicly available salary information from various government agencies. However, combining data from the three cities posed challenges, as each dataset used different formats and naming conventions. For example, similar jobs in each city were labeled differently, and some cities listed only base salaries, while others included overtime and additional pay.

To address these issues, we standardized job titles by grouping similar roles across cities into clusters and aligned key salary components such as base salary, overtime, and other pay types. After cleaning and transforming the data, we successfully built a unified dataset that allows for meaningful comparisons of salary trends across the three cities. This analysis offers valuable insights into how government compensation varies across regions and provides a clearer picture for those considering public sector careers in New York, Philadelphia, and San Francisco.

Our hypothesis going into this project is that employee compensation depends on city and job title. The following two null hypotheses capture the major research questions we are trying to answer:

- **H₀**: Average salary for all government jobs is equal  
- **H₀**: There is no difference between the average salaries for similar government jobs across these three different cities

---

## Data Organization

To prepare our dataset for analysis and modeling, we used government employee compensation data from three U.S. cities: **Philadelphia**, **New York City**, and **San Francisco**. Each city’s dataset had a different structure and naming convention, so we followed a detailed cleaning and standardization process to combine them into a single usable dataset.

We began by uploading the three raw CSV files into our Google Colab environment. These files were titled: `Philadelphia Dataset.csv`, `NEWYORK_Data.csv`, and `SanFrancisco Dataset.csv`. Each file contained information about public sector jobs, including salaries, job titles, and departments. However, the columns were labeled differently across the datasets, so our first task was to extract only the relevant information.

For each dataset, we selected five key columns that were important for our research questions: the employee’s **job title**, the **department** they worked in, their **base salary**, any **overtime pay**, and any **other forms of compensation** such as bonuses. We then standardized the column names to be consistent across all three datasets. For example, even if one city called it "Salaries" and another called it "Base Salary", we renamed them all to `base_salary`. This made the datasets compatible and easy to merge.

Next, we added a new column called `city` to each dataset, indicating which city the data came from. This allowed us to later analyze trends and salary differences across cities. We labeled each row accordingly with "Philadelphia", "New York", or "San Francisco".

After this, we combined the three cleaned datasets into one using a simple `concat` function in pandas. This created a single dataset with over **2.7 million rows**, each representing a unique government employee record from one of the three cities.

Before saving the final version, we did some additional cleaning. We converted all salary-related columns to numeric data types to make sure they could be used in calculations and machine learning models. We also removed any rows where the base salary or job title was missing, since these are essential for our analysis.

Finally, we saved the cleaned and combined dataset as `placeholder.csv`. This file contains six columns: `job_title`, `department`, `base_salary`, `overtime_pay`, `other_pay`, and `city`. It served as the foundation for all of our Exploratory Data Analysis and machine learning tasks.

---

## Data Understanding - Exploratory Data Analysis (EDA)

In this project, we explore a combined dataset of government employee compensation across Philadelphia, New York City, and San Francisco. The unified dataset (placeholder.csv) includes cleaned and standardized columns derived from public payroll data in each city. Understanding the structure of this dataset is essential for conducting meaningful exploratory data analysis (EDA) and building machine learning (ML) models.

The following table provides a detailed data dictionary that outlines each variable, its data type, whether or not it contains missing values, and a clear description of what it represents. This step ensures transparency and improves the reproducibility of our analysis.

This table was generated using Plotly, which allows for a clear and aesthetically structured overview of our dataset’s schema.

Nominal: job_title, department, city
Ratio: over_time pay, base_salary, and other_pay

![Feature Importance](assets/download%20(1).png)

This graph shows which features most influenced base salary predictions in our Random Forest model. `other_pay` came out as the most important, followed by `overtime_pay`. Interestingly, `city_New York` ranked ahead of the other cities, suggesting more variation in pay within that city.

### 2. Top 10 Highest Median Salary Job Titles

![Top Median Salary Jobs](assets/download%20(2).png)

The bar chart highlights the most lucrative roles based on median base salary across the dataset. The top roles are predominantly in public health and medical services.

### 3. Top Departments by Average Overtime Pay

![Top OT Departments](assets/download%20(3).png)

The highest average overtime pay was found in Sheriff's and Emergency departments, especially those tied to public safety and critical services.

### 4. Average Compensation Breakdown by City

![Comp Breakdown](assets/download%20(4).png)

This stacked bar chart breaks down how much of total pay comes from base salary, overtime, and other pay. NYC shows a heavier reliance on `other_pay`.

The salary distribution in Philadelphia reveals a clear and well-structured compensation pattern. The histogram shows two distinct peaks, indicating a bimodal distribution. The first and slightly lower peak appears around the 60,000–70,000 range, while the second and more prominent peak is situated around 90,000–100,000. This suggests that a large portion of employees fall into two major salary bands, likely corresponding to different job classifications or tiers of government service. These could represent, for instance, unionized administrative positions at the lower mode and professional or supervisory roles at the higher one.

The distribution for New York appears broadly dispersed, with visible clustering around 60,000–100,000, and several smaller peaks across the range. These more meaningful portions of the histogram resemble the structured pay bands seen in other cities like Philadelphia, indicating the presence of formal pay grades. However, their visibility is heavily muted by the distortion caused by the zeros.

The salary distribution in San Francisco is both rich and complex, showing a broad range of earnings that sets it apart from Philadelphia and New York. Unlike the latter two cities, San Francisco displays a right-skewed distribution with a long tail extending beyond 200,000, and in some cases, well past 400,000. This indicates the presence of a significant number of high-income earners, possibly reflecting the city’s concentration of highly compensated public-sector roles such as physicians, police/fire overtime-heavy positions, or senior technical staff. The bulk of salaries appears concentrated in the 60,000 to 120,000 range, with visible clustering around 80,000 to 100,000. This aligns with what we might expect from a structured public payroll system in a high-cost urban area — many roles fall within a middle-to-upper band, likely due to local living costs and union contracts. The relatively large spread in salary values — from just above 20,000 to well beyond 300,000 — suggests a diverse set of roles within the dataset, ranging from entry-level clerical positions to high-level management or specialized staff.

### 5. Base Salary Boxplot by City

![Boxplot](assets/download%20(5).png)

The box plot clearly confirms the hypothesis that San Francisco has the highest median base salary among the three cities. Its salary distribution is centered significantly higher than those of New York and Philadelphia, reflecting the city’s elevated cost of living and the need for competitive public sector compensation. While Philadelphia shows the lowest median and a relatively narrow, compressed distribution—indicative of a more standardized pay structure—New York has a broader spread with a median slightly above Philadelphia's but still well below San Francisco’s. Notably, New York's wider range and lower whiskers reflect earlier observations of anomalously low or missing base salary entries, possibly compensated through other pay. San Francisco also displays the largest number of high-end outliers, suggesting the presence of extremely well-compensated roles or data inconsistencies. Overall, the box plot supports the idea that San Francisco leads in base salary, while Philadelphia maintains a rigid pay scale, and New York’s structure is more varied and potentially data-skewed.

Having studied the distribution of base salaries, we turn our attention to total compensation. In analyzing public sector compensation, it's not enough to look at base salary alone. Employees may receive significant earnings from overtime or other pay components. Understanding the proportion of each component in the total compensation provides insight into how cities structure pay, manage staffing, and rely on variable compensation. This breakdown can also uncover departments or job roles where base pay is low, but total earnings are heavily supplemented by other means. We begin by making the assumption that "In most cities, the majority of an employee's total compensation comes from base salary, with overtime and other pay contributing less than 30% combined."

We then tabulate this data to actually show the percentages of the total compesation each compensation type contributes.

### 6. Smoothed Salary Distribution

![KDE](assets/download%20(6).png)

Philadelphia and New York both exhibit bimodal distributions, suggesting two primary salary bands within those cities, with peaks around 40K–90K. San Francisco displays a broader, unimodal distribution, with a peak around $75K and a long right tail, indicating a wider salary range and more high earners. The density is overall low, reflecting the smoothing and normalization across a large salary range.
We also want to know which cities have higher base salaries than the others.San Francisco is likely to have higher median base salary than both New York and Philadelphia, due to its elevated cost of living and the need to offer competitive compensation to attract and retain skilled public employees, especially in high-demand roles like tech, healthcare, and engineering. We would like to show this by running a box plot.

### 7–9. City-Specific Salary Histograms

![SF Histogram](assets/download%20(7).png)

The stacked bar plot reveals that while base salary dominates total compensation across all three cities, there are noteworthy differences in the proportions of overtime pay and other pay that suggest distinct compensation practices. Overtime pay remains relatively modest in all cities, making up roughly 5–8% of total compensation, with slightly higher shares in Philadelphia and San Francisco, likely due to staffing models in departments like public safety or healthcare. However, the standout observation is New York’s unusually high reliance on “other pay,” which constitutes nearly 25% of total compensation—substantially more than in Philadelphia or San Francisco, where it remains under 5%. This significant difference hints at New York’s use of non-standardized or discretionary compensation mechanisms, such as bonuses, stipends, or adjustments possibly used to supplement $0 base salaries (as previously observed in the data). The elevated “other pay” in New York, when combined with modest overtime usage, suggests a pay system that is more variable and potentially less transparent than in the other two cities, and it warrants further investigation into departmental or role-specific compensation structures.

Double-click (or enter) to edit
![NY Histogram](assets/download%20(8).png) 

The bar plot reveals that the Sheriff's Department stands out as the top department in terms of average overtime pay, with values exceeding $32,000 per employee. This is closely followed by the Fire Department and Police, both of which are traditionally known for shift-based, around-the-clock operations that require frequent overtime coverage. These results are consistent with expectations, as public safety departments often depend on overtime to fill coverage gaps, accommodate emergencies, or manage understaffing.

Other departments that appear in the top 10, such as Emergency Management, Emergency Communications, and Sanitation, also reflect roles that support critical infrastructure and may operate outside of regular business hours. These roles often experience spikes in demand (e.g., during storms, emergencies, or service disruptions), further explaining their high average overtime.
![Philly Histogram](assets/download%20(9).png)

Histograms confirm New York and San Francisco have more spread and upper tails in compensation compared to Philadelphia.

---

## Machine Learning Insights

### LightGBM Salary Prediction Output

![ML Output Summary](assets/download%20(10).png)

The LightGBM model achieved an R² score of ~0.716, showing decent predictive power for salary using `job_title`, `city`, `overtime_pay`, and `other_pay`.

`other_pay` dominated feature importance, reinforcing our EDA findings that high-paying jobs often include large "other" compensation — such as bonuses, stipends, or retirement payouts.

---

## Project Resources

📁 **Data Repository (Google Drive)**: [Access the full dataset](https://drive.google.com/drive/folders/1HWVh9lLD9AbmIfwc7M2vn55fbR9ZKStM?usp=drive_link)  
📹 **5-Minute Video Presentation**: [Watch here](https://your-presentation-link.com)  
📹 **Google Colab Tutorial Walkthrough**: [Watch walkthrough](https://youtu.be/95L-Ps0Roj4)  
📊 **Project Slides**: [Google Slides](https://docs.google.com/presentation/d/1hezgQOHoLfX9G5gEsrmQ3CcrQoJRH5h65-TzNkej9No/edit?usp=sharing)

---

## Acknowledgements

Thanks to:

- **Professor Lopez**, DS-201, Lafayette College
- Open-source libraries: pandas, seaborn, scikit-learn, LightGBM
- City of Philadelphia, New York, and San Francisco for transparency
