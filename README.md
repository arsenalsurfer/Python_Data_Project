# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles.This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

# The Questions

Below are the question I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand and High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I harneed the power of several key tools:

- Python: The backbone of the analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
  - Panda Library: This was used to analyze the data
  - Matplotlib Library: I visualized the data.
  - Seaborn Library: Helped me create more advanced visuals.
- Jupyter Notebooks: The tool I used to run Python scripts which let me easily include my notes and analysis.
- Visual Studio Code: My go-to for executing my Python Scripts
- Git & GitHub; Essential for version control and sharing my Python code and analysis.

# Data Perparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and reusability.

## Import and Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by inital data cleaning such as converting string to dates.

# The Analysis

## 1. What are the most demanded skills for the top3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popula, andgot the top 5 skills for these top 3 roles. this query highlights the most popular job titles and their top skills, showing which skills I shouldpay attention to depending on the role I'm targeting.

View my notebook with detailed steps here:
[Skill_Demand.ipynb](DA_Python_Data_Jobs_Project\2_Skills_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
  df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
  # df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=job_title)
  sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
  sns.despine() # remove the border
  ax[i].set_title(job_title)
  ax[i].set_ylabel('')
  ax[i].set_xlabel('')
  ax[i].get_legend().remove()
  ax[i].set_xlim(0, 78)

  for n, v in enumerate(df_plot['skill_percent']):
    ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

  if i != len(job_titles) -1:
    ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()

```

### Results

![1_Visualizations of Top SKills for Data Nerds](DA_Python_Data_Jobs_Project\images\2_skill_demand_all_data_roles.png)

### Insights

- Python is a versatile skill, highly demanded accross all three roles, but most prominently for Data Scientist (72%) and Data Engineers (65%).
- SQL is the most requested skills for Data Analysts and Data Scientist, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) comparedto Data Analysts and Data Scientists who areexpected to be proficient in more general datamanagement and analysis tools (Excel, Tableau).

# The Analysis

## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data

```python

df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.title('Trending Top Skills for Data Analyst in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

for i in range(5):
  plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()

```

### Results

![2_Trending Top Skills for Data Analysts in the US](DA_Python_Data_Jobs_Project\images\1_trending_top_skills_for_data_Analysts_in_the_US.png)

_Bar graph visualizing the trending top skills for data analysts in the US in 2023._

### Insights:

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctutations but remain essential skills for data analysts.Power BI, while less demanded compared to theothers, shows a slight upward trend toward the year's end.

# The Analysis

## 3. How well do jobs and skills pay for the Data Analysts?

### Salary Analisys for Data Persons

```python

sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
#plt.boxplot(job_list, labels=job_titles, vert=False)
plt.title('Salary Distribution in the United States')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.xlim(0, 600000)

plt.show()

```

### Results

![3_Project/images/salary_boxplot.png](DA_Python_Data_Jobs_Project\images\3_how_well_do_jobs_and_skills_pay_for_Data_Analysts.png)

_Box plot visualizong the salary distribution for the top 6 data job titles_

### Insights:

- There's a significant variation in salary ranges across different job titles. Senior Data Scientistpositions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.
- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or ciscumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstate more consistency in salary, with fewer outliers.
- The median salaries increase with the seniority and specializations of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

# The Analysis

## 4. How well do jobs and skills pay for the Data Analysts?

### Higest Paid & Most Demanded Skills for Data Analyst

#### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

# Color Palette: Sequential, Diverging, Qualitative
sns.set_theme(style='ticks')
#df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False)
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r') # to reverse color palette add _r in the end of the color
ax[0].legend().remove()

ax[0].set_title('Top 10 Higest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

#df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].legend().remove()

ax[1].set_title('Top 10 Most in Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


fig.tight_layout()


```

#### Results

![4_highest_paid_and_most_demand_skills.png](DA_Python_Data_Jobs_Project\images\4_highest_paid_and_most_demand_skills.png)

_Two seperate bar graphs visualizing the highest paid skills and most in-demand skills for data analyst in the US_

#### Insights:

- The top graph shows specialized technical skills like `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.
- The bottom graph highlights that foundational skills like `Excel`, `Powerpoint`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.
- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data Analysts aiming to maximize their career potential should consider developing a diverese skill set that includes both high-paying diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

# The Analysis

## 5. What is the most optimal skill to learn for Data Analysts?

#### Visualize Data

```python

from adjustText import adjust_text

#df_plot.plot(kind='scatter', x='skill_percent', y='median_salary', figsize=(8,8))
sns.set_theme(style='ticks')
sns.scatterplot(
  data=df_plot,
  x='skill_percent',
  y='median_salary',
  hue='technology'
)
sns.despine()

# created a empty list where we will append the texts list that we will use for adjust_Text function
texts = []

# Plotting with Matplotlib giving text
for i, txt in enumerate(df_plot['skills']):
  print(txt)
  texts.append(plt.text(x=df_plot['skill_percent'].iloc[i], y=df_plot['median_salary'].iloc[i], s=txt))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray', lw=1))
from matplotlib.ticker import PercentFormatter
# this is where we declare the ax=plt.gca()
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.xlabel('Count of Job Postings')
plt.ylabel('Percent of Data Analysts Jobs')
plt.title('Most Optimal Skills for Data Analysts in the US', fontsize=15)

plt.tight_layout()
plt.show()

```

### Results

![5_optimal_skills.png](DA_Python_Data_Jobs_Project\images\5_optimal_skills.png)

_A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US_

#### Insights:

- The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to
  other categories, indicating thgat programming expertise might offer greater salary benefits within the data analytics fileds.
- Analyst tolls (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualizations and data analysis software are crucial for current job roles. This category not only has good salaries but is also versatile across different types of data tasks.
- The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

# Challenge I faced

- Data Inconsistencies
- Complex Data Visualization
- Balancing Breadth and Depth: Deciding how deeply to dive into each analysis.

# Conclusion

This exploration into the data analysts job market has been incredibly informative, highligthing the critical skills and trends that shape this evloving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytic, This project is a good foundation for future explorations and underscores the importance of continous learning and adaptation in the data field.
