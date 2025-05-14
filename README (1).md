
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

### 1. Feature Importance Focused on City Effects

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

### 5. Base Salary Boxplot by City

![Boxplot](assets/download%20(5).png)

This boxplot confirms San Francisco offers the highest median base salary. Philadelphia shows the lowest, with New York in between but with more spread and outliers.

### 6. Smoothed Salary Distribution

![KDE](assets/download%20(6).png)

San Francisco has a more normal distribution, while Philadelphia and New York have more clearly bimodal shapes, potentially representing salary tiers.

### 7–9. City-Specific Salary Histograms

![SF Histogram](assets/download%20(7).png)
![NY Histogram](assets/download%20(8).png)
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
