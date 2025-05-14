# Employee Compensation Case Study

## By Biose Ugbo, Mauirce Otieno, Yoryi Roque, and Wanos Bahiru

### Introduction

As college students and aspiring data scientists, we are curious about salary variations across different roles and cities. A key decision young professionals face when starting their careers is choosing where to work. To explore this, we focused on New York City, Philadelphia, and San Francisco, major urban centers with extensive government jobs that offer diverse employment opportunities. These cities are attractive destinations for young professionals and provide a rich dataset of public sector salaries.

Government jobs are typically stable and offer a wide range of positions. Their compensation data is also more transparent and standardized compared to private sector roles, which makes it easier to analyze salary distributions. We collected the data from  [SanFrancisco ](https://catalog.data.gov/dataset/employee-compensation), [New York](https://catalog.data.gov/dataset/citywide-payroll-data-fiscal-year), and [Philadelphia](
https://catalog.data.gov/dataset/city-employee-earnings) which houses publicly available salary information from various government agencies. However, combining data from the three cities posed challenges, as each dataset used different formats and naming conventions. For example, similar jobs in each city were labeled differently, and some cities listed only base salaries, while others included overtime and additional pay.

To address these issues, we standardized job titles by grouping similar roles across cities into clusters and aligned key salary components such as base salary, overtime, and other pay types. After cleaning and transforming the data, we successfully built a unified dataset that allows for meaningful comparisons of salary trends across the three cities. This analysis offers valuable insights into how government compensation varies across regions and provides a clearer picture for those considering public sector careers in New York, Philadelphia, and San Francisco.

Our hypothesis going into this project is that employee compnesation depends on city and job title. The following two null hypothesis capture the major research question we are trying to answer.

Ho: Average salary for all government jobs is equal  
Ho: There is no differnce between the average salaries for similar government jobs across these three different cities



```
# This is formatted as code
```



# Data Organization

To prepare our dataset for analysis and modeling, we used government employee compensation data from three U.S. cities: **Philadelphia**, **New York City**, and **San Francisco**. Each city’s dataset had a different structure and naming convention, so we followed a detailed cleaning and standardization process to combine them into a single usable dataset.

We began by uploading the three raw CSV files into our Google Colab environment. These files were titled: `Philadelphia Dataset.csv`, `NEWYORK_Data.csv`, and `SanFrancisco Dataset.csv`. Each file contained information about public sector jobs, including salaries, job titles, and departments. However, the columns were labeled differently across the datasets, so our first task was to extract only the relevant information.

For each dataset, we selected five key columns that were important for our research questions: the employee’s **job title**, the **department** they worked in, their **base salary**, any **overtime pay**, and any **other forms of compensation** such as bonuses. We then standardized the column names to be consistent across all three datasets. For example, even if one city called it "Salaries" and another called it "Base Salary", we renamed them all to `base_salary`. This made the datasets compatible and easy to merge.

Next, we added a new column called `city` to each dataset, indicating which city the data came from. This allowed us to later analyze trends and salary differences across cities. We labeled each row accordingly with "Philadelphia", "New York", or "San Francisco".

After this, we combined the three cleaned datasets into one using a simple `concat` function in pandas. This created a single dataset with over **2.7 million rows**, each representing a unique government employee record from one of the three cities.

Before saving the final version, we did some additional cleaning. We converted all salary-related columns to numeric data types to make sure they could be used in calculations and machine learning models. We also removed any rows where the base salary or job title was missing, since these are essential for our analysis.

Finally, we saved the cleaned and combined dataset as `placeholder.csv`. This file contains six columns: `job_title`, `department`, `base_salary`, `overtime_pay`, `other_pay`, and `city`. It served as the foundation for all of our Exploratory Data Analysis and machine learning tasks.

This cleaning process was essential to ensure consistency, accuracy, and usability across all parts of our project.

```python
# Install gdown if you haven't already
# !pip install gdown

import gdown
import pandas as pd

# Google Drive file ID
file_id = '1_qpjeiKqN3txQH7B-r2RhS21ccEcVB0f'
url = f'https://drive.google.com/uc?id={file_id}'

# Download the file with gdown
output = 'combined_city_salaries.csv'
gdown.download(url, output, quiet=False)

# Load the CSV
df_all = pd.read_csv(output)


# Convert salary columns to numeric
df_all['base_salary'] = pd.to_numeric(df_all['base_salary'], errors='coerce')
df_all['overtime_pay'] = pd.to_numeric(df_all['overtime_pay'], errors='coerce')
df_all['other_pay'] = pd.to_numeric(df_all['other_pay'], errors='coerce')

# Drop rows with missing key values
df_all = df_all.dropna(subset=['base_salary', 'job_title'])

# Final preview

df_all.head()
# Save the final cleaned and merged file
df_all.to_csv("placeholder.csv", index=False)

# Optional: download it to your computer directly from Colab
from google.colab import files
files.download("placeholder.csv")
# The number of data enteries in each city
df_all['city'].value_counts()
df = pd.read_csv("placeholder.csv")
```

Here's a peak into the first few entries of our data set.

```python
df_all.head()
```

# Data Understanding



In this project, we explore a combined dataset of government employee compensation across Philadelphia, New York City, and San Francisco. The unified dataset (placeholder.csv) includes cleaned and standardized columns derived from public payroll data in each city. Understanding the structure of this dataset is essential for conducting meaningful exploratory data analysis (EDA) and building machine learning (ML) models.

The following table provides a detailed data dictionary that outlines each variable, its data type, whether or not it contains missing values, and a clear description of what it represents. This step ensures transparency and improves the reproducibility of our analysis.

This table was generated using Plotly, which allows for a clear and aesthetically structured overview of our dataset’s schema.

Nominal: job_title, department, city    
Ratio: over_time pay, base_salary, and other_pay    

```python
import pandas as pd
import plotly.graph_objects as go

# Load your dataset
df = pd.read_csv("placeholder.csv")

# Prepare table components
attributes = df.columns.tolist()
data_types = df.dtypes.astype(str).tolist()
nullables = ['Yes' if df[col].isnull().any() else 'No' for col in attributes]

# Manually define descriptions
descriptions = [
    "Job classification/title",               # job_title
    "Department or agency name",              # department
    "Base yearly salary (gross)",             # base_salary
    "Earnings from overtime work",            # overtime_pay
    "Other compensation (e.g., bonuses)",     # other_pay
    "City the employee works in"              # city
]

# Manually define levels of measurement
Data_types = [
    "Ordinal",    # job_title
    "Ordinal",    # department
    "Ratio",      # base_salary
    "Ratio",      # overtime_pay
    "Ratio",      # other_pay
    "Ordinal"     # city
]

# Ensure all lists are same length
assert len(attributes) == len(data_types) == len(nullables) == len(descriptions) == len(Data_types)

# Set up table visual
headerColor = 'slateblue'
rowEvenColor = 'lavender'
rowOddColor = 'white'

fig = go.Figure(data=[go.Table(
    columnorder=[1, 2, 3, 4, 5],
    columnwidth=[450, 300, 600, 300, 320],  # Increased widths
    header=dict(
        values=[
            '<b>Attribute</b>',
            '<b>Data Type</b>',
            '<b>Description</b>',
            '<b>Nullable</b>',
            '<b>Level of Measurement</b>'
        ],
        line_color='darkslategray',
        fill_color=headerColor,
        align=['left', 'center'],
        font=dict(color='white', size=13)
    ),
    cells=dict(
        values=[attributes, data_types, descriptions, nullables, Data_types],
        line_color='darkslategray',
        fill_color=[[rowOddColor, rowEvenColor]*3],
        align=['left', 'center'],
        font=dict(color='darkslategray', size=13)
    )
)])

# Make the figure larger
fig.update_layout(width=1200, height=600)

fig.show()
```

#Exploratory Data Analysis

In this section we would like to understand certain aspects of our data. We begin by looking at the distribution of salaries for all the cities. However all 3 of the datasets contain values at or around 0 dollars. What this causes is a huge amount of data points stored at or around the zero value, which affects potential analysis of the data to skew in favor of the very small values. Since our focus is not on these values, and we wish to have them not affect the EDA, the best option is to first check how many values are located in between and then eliminate a small range of the values. In this case, all workers earning less than $500 will be removed from the data set for cleaning.

```python
import pandas as pd
from IPython.display import display

# Filter for salaries between 0 and 500
low_salary_df = df[(df['base_salary'] > 0) & (df['base_salary'] <= 500)]

# Group by city and count
low_salary_counts = low_salary_df.groupby('city').size().reset_index(name='count')
```

```python
# Filter to relevant columns and remove rows with zero or missing base_salary
df = df[['city', 'base_salary']].dropna()
df = df[df['base_salary'] >= 500]  # Remove rows where base_salary is 0 or less
```

We begin our analysis of the distribution of the data by plotting histograms for each city in the dataset for salaries. For each unique city, it creates a histogram with using Seaborn, where the X-axis represents base salary ranges and the Y-axis shows the number of employees in each range. Each city’s histogram is plotted in its own figure, allowing for an individual view of how salaries are distributed across employees within that city. The result helps identify patterns such as concentration of pay ranges, presence of outliers, or salary skewness specific to each city.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Set style for seaborn
sns.set(style="whitegrid")

# Filter to relevant columns
df = df[['city', 'base_salary']].dropna()

# Create histograms by city
cities = df['city'].unique()

# Set up plot
for city in cities:
    plt.figure(figsize=(10, 6))
    city_data = df[df['city'] == city]

    # Plot histogram
    sns.histplot(
        city_data['base_salary'],
        bins=100,
        kde=False,
        stat='count'  # 'count' ensures x-axis is number of data points
    )

    plt.title(f'Salary Distribution in {city}')
    plt.xlabel('Base Salary')
    plt.ylabel('Number of Employees')
    plt.tight_layout()
    plt.show()
```

The salary distribution in Philadelphia reveals a clear and well-structured compensation pattern. The histogram shows two distinct peaks, indicating a bimodal distribution. The first and slightly lower peak appears around the 60,000–70,000 range, while the second and more prominent peak is situated around 90,000–100,000. This suggests that a large portion of employees fall into two major salary bands, likely corresponding to different job classifications or tiers of government service. These could represent, for instance, unionized administrative positions at the lower mode and professional or supervisory roles at the higher one.

The distribution for New York appears broadly dispersed, with visible clustering around 60,000–100,000, and several smaller peaks across the range. These more meaningful portions of the histogram resemble the structured pay bands seen in other cities like Philadelphia, indicating the presence of formal pay grades. However, their visibility is heavily muted by the distortion caused by the zeros.

The salary distribution in San Francisco is both rich and complex, showing a broad range of earnings that sets it apart from Philadelphia and New York. Unlike the latter two cities, San Francisco displays a right-skewed distribution with a long tail extending beyond 200,000, and in some cases, well past 400,000. This indicates the presence of a significant number of high-income earners, possibly reflecting the city’s concentration of highly compensated public-sector roles such as physicians, police/fire overtime-heavy positions, or senior technical staff. The bulk of salaries appears concentrated in the 60,000 to 120,000 range, with visible clustering around 80,000 to 100,000. This aligns with what we might expect from a structured public payroll system in a high-cost urban area — many roles fall within a middle-to-upper band, likely due to local living costs and union contracts. The relatively large spread in salary values — from just above 20,000 to well beyond 300,000 — suggests a diverse set of roles within the dataset, ranging from entry-level clerical positions to high-level management or specialized staff.

## Smoothed Salary distribution

Philadelphia and New York both exhibit bimodal distributions, suggesting two primary salary bands within those cities, with peaks around 40K–90K. San Francisco displays a broader, unimodal distribution, with a peak around $75K and a long right tail, indicating a wider salary range and more high earners. The density is overall low, reflecting the smoothing and normalization across a large salary range.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load and clean data
df = pd.read_csv("placeholder.csv")
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df = df[['city', 'base_salary']].dropna()
df = df[df['base_salary'] >= 20000]

# Filter to specific cities
cities_to_plot = ['Philadelphia', 'New York', 'San Francisco']
df = df[df['city'].isin(cities_to_plot)]

# Plot KDE for each city with labeled legend
plt.figure(figsize=(12, 6))
for city in cities_to_plot:
    sns.kdeplot(
        data=df[df['city'] == city],
        x='base_salary',
        label=city,
        bw_adjust=2.0
    )

plt.title('Smoothed Salary Distribution: Philadelphia, New York, San Francisco')
plt.xlabel('Base Salary ($)')
plt.ylabel('Density')
plt.xlim(20000, 300000)

# Adjust legend position and layout
plt.legend(title='City', loc='center left', bbox_to_anchor=(1.02, 0.5))
plt.grid(True)
plt.tight_layout(rect=[0, 0, 0.85, 1])
plt.show()
```

We also want to know which cities have higher base salaries than the others.San Francisco is likely to have higher median base salary than both New York and Philadelphia, due to its elevated cost of living and the need to offer competitive compensation to attract and retain skilled public employees, especially in high-demand roles like tech, healthcare, and engineering. We would like to show this by running a box plot.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
df = pd.read_csv("placeholder.csv")

# Clean and prepare
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df = df.dropna(subset=['base_salary', 'city'])

# Plot: Boxplot of Base Salary by City
plt.figure(figsize=(10, 6))
sns.boxplot(x='city', y='base_salary', data=df, palette='pastel')
plt.title('Base Salary Distribution by City')
plt.xlabel('City')
plt.ylabel('Base Salary')
plt.tight_layout()
plt.show()
```

The box plot clearly confirms the hypothesis that San Francisco has the highest median base salary among the three cities. Its salary distribution is centered significantly higher than those of New York and Philadelphia, reflecting the city’s elevated cost of living and the need for competitive public sector compensation. While Philadelphia shows the lowest median and a relatively narrow, compressed distribution—indicative of a more standardized pay structure—New York has a broader spread with a median slightly above Philadelphia's but still well below San Francisco’s. Notably, New York's wider range and lower whiskers reflect earlier observations of anomalously low or missing base salary entries, possibly compensated through other pay. San Francisco also displays the largest number of high-end outliers, suggesting the presence of extremely well-compensated roles or data inconsistencies. Overall, the box plot supports the idea that San Francisco leads in base salary, while Philadelphia maintains a rigid pay scale, and New York’s structure is more varied and potentially data-skewed.

Having studied the distribution of base salaries, we turn our attention to total compensation. In analyzing public sector compensation, it's not enough to look at base salary alone. Employees may receive significant earnings from overtime or other pay components. Understanding the proportion of each component in the total compensation provides insight into how cities structure pay, manage staffing, and rely on variable compensation. This breakdown can also uncover departments or job roles where base pay is low, but total earnings are heavily supplemented by other means. We begin by making the assumption that "In most cities, the majority of an employee's total compensation comes from base salary, with overtime and other pay contributing less than 30% combined."

We then tabulate this data to actually show the percentages of the total compesation each compensation type contributes.

```python
# Load and clean
df = pd.read_csv("placeholder.csv")
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df['overtime_pay'] = pd.to_numeric(df['overtime_pay'], errors='coerce')
df['other_pay'] = pd.to_numeric(df['other_pay'], errors='coerce')

# Drop rows with missing or zero total compensation
df_clean = df.dropna(subset=['base_salary', 'overtime_pay', 'other_pay']).copy()
df_clean['total_comp'] = df_clean['base_salary'] + df_clean['overtime_pay'] + df_clean['other_pay']
df_clean = df_clean[df_clean['total_comp'] > 0].copy()

# Calculate proportions safely
df_clean['base_pct'] = df_clean['base_salary'] / df_clean['total_comp']
df_clean['ot_pct'] = df_clean['overtime_pay'] / df_clean['total_comp']
df_clean['other_pct'] = df_clean['other_pay'] / df_clean['total_comp']

# Build summary table
summary_df = pd.DataFrame({
    'Component': ['Base Salary', 'Overtime Pay', 'Other Pay'],
    'Average Share (%)': [
        df_clean['base_pct'].mean() * 100,
        df_clean['ot_pct'].mean() * 100,
        df_clean['other_pct'].mean() * 100
    ]
})

# Display the result
summary_df.style.format({'Average Share (%)': '{:.2f}'})
summary_df.style.format({'Average Share (%)': '{:.2f}'})
```

The results support our hypothesis. On average, base salary comprises over 80% of total compensation, while overtime and other pay together account for less than 20%. This confirms that most cities rely primarily on structured base pay for compensation, with variable elements playing a secondary role. However, some cities may rely on other forms of pay to supplement its employee compnesation.
We run an analysis that helps identify which cities might be over-relying on overtime, which can have implications for employee burnout, labor relations, and budget predictability.

```python
import pandas as pd
import matplotlib.pyplot as plt
import warnings
warnings.simplefilter(action='ignore', category=pd.errors.SettingWithCopyWarning)


# Load and clean data
df = pd.read_csv("placeholder.csv")
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df['overtime_pay'] = pd.to_numeric(df['overtime_pay'], errors='coerce')
df['other_pay'] = pd.to_numeric(df['other_pay'], errors='coerce')

df = df.dropna(subset=['base_salary', 'overtime_pay', 'other_pay', 'city'])
df['total_comp'] = df['base_salary'] + df['overtime_pay'] + df['other_pay']
df = df[df['total_comp'] > 0]

# Compute proportions
df['base_pct'] = df['base_salary'] / df['total_comp']
df['ot_pct'] = df['overtime_pay'] / df['total_comp']
df['other_pct'] = df['other_pay'] / df['total_comp']

# Calculate and prepare composition data
city_composition = df.groupby('city')[['base_pct', 'ot_pct', 'other_pct']].mean()
city_composition = city_composition[['base_pct', 'ot_pct', 'other_pct']]
city_composition.columns = ['Base Salary', 'Overtime Pay', 'Other Pay']

# Plot only the stacked bar chart
city_composition.plot(
    kind='bar',
    stacked=True,
    figsize=(10, 6),
    colormap='tab10'
)

plt.title('Average Compensation Breakdown by City')
plt.ylabel('Proportion of Total Compensation')
plt.xlabel('City')
plt.ylim(0, 1)
plt.legend(loc='upper right')
plt.tight_layout()
plt.show()
```

The stacked bar plot reveals that while base salary dominates total compensation across all three cities, there are noteworthy differences in the proportions of overtime pay and other pay that suggest distinct compensation practices. Overtime pay remains relatively modest in all cities, making up roughly 5–8% of total compensation, with slightly higher shares in Philadelphia and San Francisco, likely due to staffing models in departments like public safety or healthcare. However, the standout observation is New York’s unusually high reliance on “other pay,” which constitutes nearly 25% of total compensation—substantially more than in Philadelphia or San Francisco, where it remains under 5%. This significant difference hints at New York’s use of non-standardized or discretionary compensation mechanisms, such as bonuses, stipends, or adjustments possibly used to supplement $0 base salaries (as previously observed in the data). The elevated “other pay” in New York, when combined with modest overtime usage, suggests a pay system that is more variable and potentially less transparent than in the other two cities, and it warrants further investigation into departmental or role-specific compensation structures.



```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
df = pd.read_csv("placeholder.csv")

# Clean salary fields
df['overtime_pay'] = pd.to_numeric(df['overtime_pay'], errors='coerce')
df['department'] = df['department'].astype(str)

# Drop rows with missing or null overtime or department
df = df.dropna(subset=['overtime_pay', 'department'])
df = df[df['department'].str.lower() != 'nan']  # Remove literal 'nan' as string

# Standardize fire department names
df['department'] = df['department'].replace({
    'FIRE DEPARTMENT': 'Fire Department',
    'FIR Fire Department': 'Fire Department'
})

# Group and plot
avg_overtime_by_dept = df.groupby('department')['overtime_pay'].mean().sort_values(ascending=False).head(10)

# Plot
plt.figure(figsize=(12, 6))
sns.barplot(x=avg_overtime_by_dept.values, y=avg_overtime_by_dept.index, color='steelblue')
plt.title('Top 10 Departments by Average Overtime Pay (Cleaned)')
plt.xlabel('Average Overtime Pay ($)')
plt.ylabel('Department')
plt.tight_layout()
plt.show()
```

The bar plot reveals that the Sheriff's Department stands out as the top department in terms of average overtime pay, with values exceeding $32,000 per employee. This is closely followed by the Fire Department and Police, both of which are traditionally known for shift-based, around-the-clock operations that require frequent overtime coverage. These results are consistent with expectations, as public safety departments often depend on overtime to fill coverage gaps, accommodate emergencies, or manage understaffing.

Other departments that appear in the top 10, such as Emergency Management, Emergency Communications, and Sanitation, also reflect roles that support critical infrastructure and may operate outside of regular business hours. These roles often experience spikes in demand (e.g., during storms, emergencies, or service disruptions), further explaining their high average overtime.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("placeholder.csv")

# Clean and prepare data
df = df.dropna(subset=['base_salary', 'job_title'])
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df['overtime_pay'] = pd.to_numeric(df['overtime_pay'], errors='coerce')
df['other_pay'] = pd.to_numeric(df['other_pay'], errors='coerce')

# Median salary by job title
median_by_title = df.groupby('job_title')['base_salary'].median().sort_values(ascending=False).head(10)

# Bar Plot: Top 10 Paying Job Titles (in black)
plt.figure(figsize=(12,6))
sns.barplot(x=median_by_title.values, y=median_by_title.index, color='steelblue')
plt.title('Top 10 Highest Median Salary Job Titles')
plt.xlabel('Median Base Salary')
plt.ylabel('Job Title')
plt.tight_layout()
plt.show()
```

## Summary of Correlation Insights

There’s no meaningful relationship between base salary and overtime pay — they appear to operate independently.

There’s a moderate positive correlation between other pay and both base salary and overtime pay, indicating that "other pay" might be part of a general compensation trend.

All relationships are statistically significant (p-values ≈ 0), but effect size (i.e., the actual strength of the relationship) is what truly matters — and only the ones involving other pay show practical importance.

```python
import pandas as pd
from scipy.stats import spearmanr, kendalltau

# Load and clean data
df = pd.read_csv("placeholder.csv")
df['base_salary'] = pd.to_numeric(df['base_salary'], errors='coerce')
df['overtime_pay'] = pd.to_numeric(df['overtime_pay'], errors='coerce')
df['other_pay'] = pd.to_numeric(df['other_pay'], errors='coerce')

# Drop missing values and filter out small salaries
df = df.dropna(subset=['base_salary', 'overtime_pay', 'other_pay'])
df = df[df['base_salary'] > 20000]

# Spearman correlation
s1, sp1 = spearmanr(df['base_salary'], df['overtime_pay'])
s2, sp2 = spearmanr(df['base_salary'], df['other_pay'])
s3, sp3 = spearmanr(df['overtime_pay'], df['other_pay'])

# Kendall correlation
k1, kp1 = kendalltau(df['base_salary'], df['overtime_pay'])
k2, kp2 = kendalltau(df['base_salary'], df['other_pay'])
k3, kp3 = kendalltau(df['overtime_pay'], df['other_pay'])

# Display results
print("📊 Spearman Correlation")
print(f"Base Salary ↔ Overtime Pay:  ρ = {s1:.4f}, p = {sp1:.4e}")
print(f"Base Salary ↔ Other Pay:     ρ = {s2:.4f}, p = {sp2:.4e}")
print(f"Overtime Pay ↔ Other Pay:    ρ = {s3:.4f}, p = {sp3:.4e}\n")

print("📊 Kendall Correlation")
print(f"Base Salary ↔ Overtime Pay:  τ = {k1:.4f}, p = {kp1:.4e}")
print(f"Base Salary ↔ Other Pay:     τ = {k2:.4f}, p = {kp2:.4e}")
print(f"Overtime Pay ↔ Other Pay:    τ = {k3:.4f}, p = {kp3:.4e}")
```

## Hypothesis Testing

The Kruskal–Wallis test results reveal that both job title and city have a statistically significant impact on total pay. The test comparing total pay across job titles yields a very high H-statistic of 717,512.38 with a p-value of effectively 0, indicating that differences in total compensation between job titles are highly significant and not due to random variation. Similarly, the comparison across cities results in an H-statistic of 58,469.95 with a p-value of 0, confirming that pay varies meaningfully by location as well. While both factors influence total pay, the substantially higher H-value for job title suggests it has a stronger effect than city. These results underscore that an individual’s role and geographic location are key determinants of their overall earnings.

This finding is consistent with our hypothesis stated in our introduction that employee compensation is dependent acorss job titles and cities.


```python
import pandas as pd
from scipy.stats import kruskal

# Load and clean data
df = pd.read_csv("placeholder.csv")
df = df[['city', 'job_title', 'base_salary', 'overtime_pay', 'other_pay']].dropna()
df = df[df['base_salary'] > 0]

# Compute total pay
df['total_pay'] = df['base_salary'] + df['overtime_pay'] + df['other_pay']

# Limit to top categories to make Kruskal test feasible
top_jobs = df['job_title'].value_counts().nlargest(20).index
top_cities = df['city'].value_counts().nlargest(20).index
df = df[df['job_title'].isin(top_jobs) & df['city'].isin(top_cities)]

# Kruskal-Wallis test for job_title ↔ total_pay
job_groups = [group['total_pay'].values for name, group in df.groupby('job_title')]
stat_job, p_job = kruskal(*job_groups)

# Kruskal-Wallis test for city ↔ total_pay
city_groups = [group['total_pay'].values for name, group in df.groupby('city')]
stat_city, p_city = kruskal(*city_groups)

# Print results
print("📊 Kruskal-Wallis Test Results")
print(f"Total Pay vs Job Title: H = {stat_job:.4f}, p = {p_job:.4e}")
print(f"Total Pay vs City:      H = {stat_city:.4f}, p = {p_city:.4e}")
```

# Machine Learning

In this code, machine learning is applied to predict employee salaries based on factors such as job title, city, and additional compensation (e.g., overtime and other pay). The process involves preparing the data by cleaning and encoding categorical features using one-hot encoding and standardizing numerical values. A gradient boosting model called LightGBM is then trained on a sample of this processed data to learn patterns and relationships between the input features and the target salary. Once trained, the model can predict salaries for new or unseen employee profiles. By evaluating its performance using metrics like R²  we can assess how accurately the model explains variations in salary. This approach can help uncover insights about salary trends across different cities and roles and support data-driven compensation decisions.

Based on the model’s results, which achieved an R² of approximately 72%, we can draw several valuable insights. The model indicates that job title, city, and additional compensation (overtime and other pay) are meaningful predictors of base salary, but they do not capture the full picture—suggesting that other factors like experience, education, or department may play a significant role. The moderate predictive power shows that while these features help explain salary differences, there's room for improvement through enhanced data. Still, the model reveals that certain cities and job roles are consistently associated with higher or lower salaries, and that non-base compensation contributes significantly to overall pay. This type of model could be used for identifying regional pay disparities, supporting compensation planning, and flagging underpaid roles—especially when further refined with more complete data.

**Other Pay** refers to any **extra compensation** a public employee receives **that is not part of their base salary or overtime pay**.

It usually includes things like:

* **Bonuses**
* **Retroactive pay adjustments**
* **Incentives**
* **Hazard pay**
* **Severance or payout upon retirement**
* **Stipends or allowances** (e.g., for housing, uniforms, or transportation)


 Why Did It Show Up as the #1 Predictor in the ML Model?

It actually makes a lot of sense:

* Even though you're trying to **predict base salary**, the `other_pay` column **strongly correlates** with high-paying roles and seniority.
* In many cases, only **senior-level or specialty jobs** receive large "other pay" packages.
* For example: A fire chief or medical examiner might get a large **bonus** or **reimbursement** on top of their base pay.

So while `other_pay` doesn’t *cause* a higher base salary, it often **appears with jobs that already pay well**, which is why the model sees it as important.

The machine learning model identified `other_pay` as the strongest predictor of base salary. This makes intuitive sense — employees in high-ranking or specialized roles often receive additional forms of compensation such as bonuses, stipends, or one-time payouts. These forms of ‘other pay’ are strongly associated with seniority, job title, and overall earnings power.”

## ML Part1

This code builds a LightGBM regression model to predict log-transformed base salaries based on features like city, job title, overtime pay, and other pay. It first cleans and preprocesses the data by handling missing values and filtering out extreme values. The dataset is sampled to 5000 rows for efficiency, and categorical features are one-hot encoded while numerical features are standardized. The model is trained and evaluated using R² and MAE, resulting in an R² of 0.7163, indicating that the model explains 71.63% of the variance in salaries, and a MAE of 0.80, meaning the average prediction error is around 80% of the transformed salary value.

```python
# Save the final cleaned and merged file
df_all.to_csv("placeholder.csv", index=False)

# Optional: download it to your computer directly from Colab


df = pd.read_csv("placeholder.csv")
```

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.metrics import mean_absolute_error, r2_score
import lightgbm as lgb

# Load data
df = pd.read_csv("placeholder.csv")  # Replace with your actual file path

# Select relevant columns
df = df[['city', 'job_title', 'overtime_pay', 'other_pay', 'base_salary']]

# Clean missing/invalid entries
df = df.dropna(subset=['base_salary', 'city', 'job_title'])
df = df[df['base_salary'] > 0]

# Fill missing values
df['overtime_pay'] = df['overtime_pay'].fillna(0)
df['other_pay'] = df['other_pay'].fillna(0)

# Sample down for speed
df = df.sample(n=5000, random_state=42)  # 🔹 You can reduce this further if needed

# Log-transform target
df['log_base_salary'] = np.log1p(df['base_salary'])
df = df.dropna(subset=['log_base_salary'])  # Ensure no NaNs

# Define features and target
categorical_cols = ['city', 'job_title']
numeric_cols = ['overtime_pay', 'other_pay']
X = df[categorical_cols + numeric_cols]
y = df['log_base_salary']

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

# Preprocessor
preprocessor = ColumnTransformer([
    ('cat', OneHotEncoder(handle_unknown='ignore', sparse_output=False), categorical_cols),
    ('num', StandardScaler(), numeric_cols)
])

# LightGBM model pipeline
lgb_model = Pipeline(steps=[
    ('preprocessor', preprocessor),
    ('model', lgb.LGBMRegressor(n_estimators=300, learning_rate=0.05, max_depth=6, random_state=42))
])

# Train and evaluate
lgb_model.fit(X_train, y_train)
y_pred = lgb_model.predict(X_test)

# Metrics
r2 = r2_score(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)

print(f"✅ R² with LightGBM: {r2:.4f}")
```

## ML Part 2
This block of code builds a machine learning model to analyze how different cities and numeric compensation components (overtime and other pay) influence base salary. The dataset is cleaned by dropping rows with missing salary values. A random sample of 5,000 records is used to reduce memory load. The features (city, overtime pay, and other pay) are preprocessed using a column transformer that one-hot encodes the categorical city variable and scales the numeric features. A Random Forest Regressor is then trained on the data to predict base salary. After training, the model’s feature importances are extracted to determine which variables most influence salary predictions. These importances are visualized in a bar plot, helping to identify which cities or numeric variables (like overtime pay) have the greatest impact on salary outcomes. This analysis is especially useful for understanding geographic and compensation structure effects on pay.

```python
# Focus only on 'city' and numeric variables
categorical_cols = ['city']  # Removed 'job_title' and 'department'
numeric_cols = ['overtime_pay', 'other_pay']
target_col = 'base_salary'

# Drop rows with missing target
df = df.dropna(subset=[target_col])

# Limit to top cities to avoid explosion from OneHotEncoding
top_cities = df['city'].value_counts().nlargest(20).index
df['city'] = df['city'].where(df['city'].isin(top_cities), 'Other')

# Sample to manage memory
df = df.sample(n=5000, random_state=42)

# Inputs and target
X = df[categorical_cols + numeric_cols]
y = df[target_col]

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42)

# Column transformer
preprocessor = ColumnTransformer([
    ('cat', OneHotEncoder(handle_unknown='ignore', sparse_output=False), categorical_cols),
    ('num', StandardScaler(), numeric_cols)
])

# Random Forest pipeline
rf_pipeline = Pipeline([
    ('preprocessor', preprocessor),
    ('model', RandomForestRegressor(n_estimators=100, random_state=42))
])

# Train
rf_pipeline.fit(X_train, y_train)

# Get feature importances
ohe = rf_pipeline.named_steps['preprocessor'].named_transformers_['cat']
encoded_features = ohe.get_feature_names_out(categorical_cols)
all_features = list(encoded_features) + numeric_cols
importances = rf_pipeline.named_steps['model'].feature_importances_

# Create importances DataFrame
import_df = pd.DataFrame({'feature': all_features, 'importance': importances})
top_features = import_df.sort_values(by='importance', ascending=False)

# Plot
plt.figure(figsize=(10, 6))
sns.barplot(x='importance', y='feature', data=top_features, palette='cividis')
plt.title('Feature Importance Focused on City Effects')
plt.tight_layout()
plt.show()
```

