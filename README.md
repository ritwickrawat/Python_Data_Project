# Overview
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand and High Paying)

# Tools Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - Pandas Library: This was used to analyze the data.
    - Matplotlib Library: I visualized the data.
    - Seaborn Library: Helped me create more advanced visuals.
- Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.
Visual Studio Code: My go-to for executing my Python scripts.
- Git & GitHub: Essential for sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import and Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```py
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```


# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skills_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)

```py
# Creating subplots
fig, axs = plt.subplots(3, 1, figsize=(10, 15))

# Plotting for each job title
for ax, job_title in zip(axs, job_titles):
    # Filter DataFrame for the job title and get Top 5
    df_filtered = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    
    # Plotting
    sns.barplot(data=df_filtered, x='skill_count', y='job_skills', ax=ax, palette='dark:b')
    ax.set_title(f'Top 5 Skills for {job_title}')
    ax.set_xlabel('Skill Count')
    ax.set_ylabel('Skills')
    sns.despine(ax=ax)
    plt.show()
```
# Results
![Visualization of Top Skills](3_Project\images\skill_demand.png)

### Insights

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists and Data Engineers.

## 2. How are in-demand skills trending for Data Analysts?

View my notebook with detailed steps here: [3_Skills_Trend.ipynb](3_Project\3_Skills_Trend.ipynb)

### Visualize Data

```py
df_DA_pivot = df_DA_explode.pivot_table(index='job_posted_month_no', 
                          columns='job_skills', 
                          aggfunc = 'size', 
                          fill_value=0
                          )
df_DA_pivot.loc['Total'] = df_DA_pivot.sum()

df_DA_pivot = df_DA_pivot[df_DA_pivot.loc['Total'].sort_values(ascending=False).index]

df_DA_pivot = df_DA_pivot.drop('Total')

df_DA_pivot.iloc[:,:5].plot(kind='line')
```
![Trending Skills for Data Analyst](3_Project\images\Skill_trend.png)
*Bar graph visualizing the trending top skills for data analysts in 2023.*

### Insights:

Based on the provided graph of job postings data for various skills over the months, here are some insights on trending skills for Data Analysts in 2023 worldwide:

#### SQL:
- Trend: Remains the most demanded skill throughout the year.
- Insight: Essential for data management and querying, a fundamental skill for Data Analysts.

#### Excel:
- Trend: Consistently high demand, with peaks at certain months.
- Insight: Despite the rise of advanced tools, Excel remains a critical tool for data analysis due to its versatility and ease of use.

#### Python:
- Trend: High and stable demand, with slight fluctuations.
- Insight: Key for data manipulation, analysis, and machine learning tasks. Its popularity continues to grow in the data analysis field.

#### Tableau:

- Trend: Moderate demand with some peaks, indicating periodic high demand.
- Insight: Important for data visualization and business intelligence tasks, making it a valuable skill for presenting data insights.
#### Power BI:

- Trend: Lower demand compared to other skills but still significant.

- Insight: Another important tool for data visualization and business intelligence, complementing skills in Tableau

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis
View my notebook with detailed steps here: [4_Salary_Analysis.ipynb](3_Project\4_Salary_Analysis.ipynb)

#### Visualize Data

```py
sns.boxplot(data=df_top6, x='salary_year_avg', y='job_title_short', order=job_order)
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.title('Yearly Salary')
ax = plt.gca()
plt.xlim(0,300000)
plt.show()
```
![Salary Distribution](3_Project\images\salary_analysis.png)
*Box plot visualizing the salary distributions for the top 6 data job titles*

- Senior Roles: Senior Data Scientist and Senior Data Engineer have the highest median salaries with relatively tight salary ranges, indicating a smaller variation among salaries.

- Mid-level Roles: Data Scientists and Data Engineers have a moderate range of salaries with a notable number of high outliers, suggesting potential for significant earnings.

- Entry-level Role: Data Analyst has the lowest median salary but shows substantial variability, indicating opportunities for higher pay in certain cases.

- General Trend: Higher roles typically command higher median salaries and more consistent earnings, whereas lower roles, while generally paying less, show more variability and potential for higher salaries in certain cases.


### Highest Paid & Most Demanded Skills for Data Analysts
#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts:



``` py
fig, ax = plt.subplots(2,1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay,x='median', y=df_DA_top_pay.index, ax=ax

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], palette='light:b_r')

plt.show()
```

![Highest Paid and Most In-demand skills](3_Project\images\top_paid_vs_indemand.png) *Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

### Insights:


#### High-Paying Niche Skills
- **Solidity**, **Terraform**, and **DataRobot** are among the highest paying, indicating strong demand in specific niches like blockchain, cloud infrastructure, and automated machine learning.

#### Versatile and In-Demand Skills
- **Python**, **Tableau**, and **SQL** are not only highly demanded but also offer competitive salaries, making them essential for any data analyst.

#### Combination of Skills
- Learning a combination of the highest-paying and in-demand skills can provide a competitive edge. For instance, pairing Python (in-demand) with Terraform (high-paying) can be highly beneficial.

#### Data Visualization and Reporting
- Skills like **Tableau**, **Power BI**, and **PowerPoint** are crucial for presenting data insights effectively, which is a key part of a data analyst's role.

### Best Skills to Learn
- **Python**: Due to its versatility and high demand.
- **Tableau**: For effective data visualization.
- **SQL**: Essential for data querying and database management.
- **Terraform**: For those interested in cloud infrastructure and looking for high-paying opportunities.
- **Solidity**: For those looking to enter the blockchain space.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

View my notebook with detailed steps here: [5_Optimal_Skills.ipynb](3_Project\5_Optimal_Skills.ipynb)

### Visualize Data

```py
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills['skill_percentage'], df_DA_skills['median_salary'])
plt.show()
```
![Most Optimal skills for a Data Analyst](3_Project\images\optimal_skills.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts.*

### Insights

#### High-Paying and High-Demand Skills:
- **Python** and **Tableau**: These skills are not only in high demand (with a higher percentage of Data Analyst jobs requiring them) but also offer competitive median salaries. They are essential skills for any data analyst looking to maximize their job opportunities and earning potential.

#### Niche but Lucrative Skills:
- **Azure** and **Oracle**: These skills, while not as commonly required as others, offer very high median salaries. Specializing in these areas can be beneficial for data analysts looking to enter niche markets with potentially higher pay.

#### Versatile and Essential Skills:
- **SQL**: This is a fundamental skill for data analysts, widely demanded across jobs and offering a good median salary. Mastering SQL is crucial for data querying and database management.
- **Excel**: Despite being an older tool, Excel remains relevant and in demand, offering a decent median salary.

#### Data Visualization and Reporting:
- **Power BI** and **PowerPoint**: These skills are important for presenting data insights effectively. While they may not offer the highest salaries, they are essential for the communication aspect of a data analyst’s role.

#### Statistical and Analytical Skills:
- **R** and **SAS**: These are important for statistical analysis and data manipulation. They are valued skills that offer good salaries, though not as high as some of the niche technologies like Azure.

#### Recommendations for Skills to Learn:
- **Python**: Due to its versatility and high demand.
- **Tableau**: For effective data visualization.
- **SQL**: Essential for data querying and database management.
- **Azure**: For those interested in cloud infrastructure and looking for high-paying opportunities.
- **Power BI**: For presenting data insights effectively.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.