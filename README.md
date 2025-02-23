# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

This is the Pyhton version of the project. [If you're interested in the ***SQL version***, you can find it here](https://github.com/Amit-K-M/SQL_PROJECT_DATA_JOBS)
# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
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

## Filter Indian Jobs

To focus my analysis on the Indian job market, I apply filters to the dataset, narrowing down to roles based in India.

```python
df_ind = df[df['job_country'] == 'India']

```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skill_Demand](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/2_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Results

![Likelihood of Skills Requested in the Indian Job Postings](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/Images/Likelihood_of_skills_IND.png)


*Bar graph visualizing the top 3 data roles and their top 5 skills associated with each.*

### Insights:

- SQL is the most requested skill for Data Analysts (52%) and Data Engineers (68%). For Data Scientists, Python leads the demand, appearing in 70% of job postings.

- Data Engineers require more specialized technical skills (e.g., AWS, Azure, Spark), while Data Analysts and Data Scientists are expected to be proficient in more general data analysis tools (Excel, Tableau).

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (70%) and Data Engineers (61%).


## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/3_Skill_Trend.ipynb)

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_da_ind_perc.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in India](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/Images/Trending_skills_ind.png)  
*Bar graph visualizing the trending top skills for data analysts in India in 2023.*

### Insights:
- SQL remains the most consistently demanded skill throughout the year, maintaining a lead over the others with a slight dip towards the year's end.
- Power BI experienced a steady increase in demand, with a significant rise starting around September, continuing into December.
- Python and Excel demonstrate stable demand across the year with minor fluctuations, solidifying their importance as core skills for data analysts.
- Tableau shows a modest but relatively stable demand, slightly lower than Python and Excel, with a gradual recovery in the last quarter.
## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in India and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data= top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in India](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/Images/salary_distribution_IND.png)  
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

- **Salary Variation Across Roles**: There is significant variation in salary ranges across the different data job titles. This suggests that the market values certain skills and responsibilities more than others.

- **Senior Roles Command Higher Salaries**: The senior-level roles, such as Senior Data Scientist and Senior Data Engineer, tend to have the highest salary potential, with maximum salaries reaching up to $300K or more. This indicates that advanced data skills and extensive experience are highly valued in the industry. 
  
- **Salary Outliers in Senior Roles**: Both Senior Data Scientist and Senior Data Engineer positions show a considerable number of outliers on the higher end of the salary spectrum. This suggests that exceptional skills, expertise, or unique circumstances can lead to significantly higher compensation for some individuals in these roles.
  
- **Salary Consistency in Data Analyst Roles**: Compared to the senior-level roles, Data Analyst positions demonstrate more consistency in salary, with fewer outliers. This may indicate a more standardized compensation structure for this role.
  
- **Increasing Median Salaries with Seniority**: The median salaries generally increase with the seniority and specialization of the roles. Senior-level positions not only have higher median salaries but also wider differences between typical salaries, reflecting greater variance in compensation as responsibilities and expectations increase.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the India:

![The Highest Paid & Most In-Demand Skills for Data Analysts in India](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/Images/Median_salary_by_skills.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in India.*

#### Insights:

- The top graph shows that specialized technical skills like `PostgreSQL`, `PySpark`, and `Gitlab` are associated with higher salaries, some reaching up to $160K. This suggests that advanced technical proficiency in specific tools and technologies can translate to increased earning potential for data analysts in India.
  
- The bottom graph highlights that foundational skills like `Power BI`, `Tableau`, and `SQL` are the most in-demand for data analysts, even though they may not offer the highest individual salaries. This demonstrates the importance of mastering core data analysis competencies for employability in the field.

- There appears to be a clear distinction between the skills that are highest paid and those that are most in-demand. To maximize their career potential, data analysts in India may need to develop a diverse skill set that includes both high-paying specialized technical abilities as well as the widely demanded foundational data analysis skills.
  
### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_da_skills_merged,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in  India with Coloring by Technology](https://github.com/Amit-K-M/python_data_Science_project/blob/main/Projects/Images/Most_optimal_skills_da_ind.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in India with color labels for technology.*

#### Insights:

- The scatter plot shows that skills related to analyst_tools (colored orange) tend to cluster at the higher end of the salary range,Tools like `Power BI`, `Tableau`, and `Excel` play a significant role in data analysis. While `Excel` remains a staple with nearly 42% job demand, tools like `Power BI` and `Tableau` offer better pay, with median salaries around $110K–$115K annually. These tools are not as widely in demand as Excel but mastering them can lead to better-paying roles, particularly in organizations focused on business intelligence.
  
- The Programming skills (colored blue),  like `SQL` and `Python` are essential for data analysts, with `SQL` being the most in-demand skill, appearing in nearly 50% of job postings. `Python` follows closely with ~39% demand. While these skills provide a strong foundation to secure data analyst roles, they offer moderate pay, with median salaries ranging between $95K–$100K annually. They are must-have tools but may need to be paired with niche skills for higher earning potential.
  
- Cloud platforms (colored green) like `aws` and `Azure` are emerging as valuable skills, in the data analytics space. While their current demand is lower (~15–20%), they offer solid salaries of about $90K annually. As businesses continue adopting cloud-based solutions, expertise in platforms like AWS, Azure, or similar tools will become increasingly important. Combining cloud skills with programming and analyst tools can make analysts more competitive in the job market.
  
# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation:** SQL and Python are the most in-demand skills across roles, but specialized tools like MongoDB and Spark command higher salaries, reaching up to $160K, highlighting the value of advanced technical expertise.

- **Market Trends:** Tools like Power BI are seeing rising demand, while Python and Excel remain stable throughout the year. The growing focus on AWS and Azure reflects a shift toward cloud-based solutions in data analytics.

- **Economic Value of Skills:** Foundational skills like SQL and Power BI ensure job opportunities, but pairing them with niche, high-paying tools such as Spark or Cloud platforms can significantly boost earning potential.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.
