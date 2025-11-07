 # Introduction
 📊 Dive into the data job market! Focusing on data analyst roles, this project explores top paying jobs💰, in-demand skills, and 📈 where high demand meet high salary in data analytics.

 🔍 SQL Queries? Check them out here: [project_sql folder](/project_sql/)

 # Tools I Used
 
For my deep dive into the data analyst job market, I harnessed the power of several key tools:
- **SQL**: The backbone of my analysis, allowing me to query the database and unearth critical insights.

- **PostgreSQL**: The chosen database management system, ideal for handling the job posting data.

- **Visual Studio Code**: My go-to for database management and executing SQL queries.

 - **Git & GitHub**: Essential for version control and sharing my SQL scripts and analysis,ensuring collabration and project tracking. 
 # The Analysis
 Each query for this project aimed at investigating specific aspects of the data analyst job market.
 Here's how I approached each question:
 ## 1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

``` sql
SELECT
  job_id,
  job_title,
  job_location,
  job_schedule_type,
  salary_year_avg,
  job_posted_date,
  name AS company_name
FROM
   job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
  job_title_short = 'Data Analyst' AND 
  job_location = 'Anywhere'
  and salary_year_avg IS NOT NULL
ORDER BY
   salary_year_avg DESC
LIMIT
   10.
   ```

Here's the breakdown of the top data analyst jobs in 2023:

- **Wide Salary Range:** Top 10 paying data
analyst roles span from $184,000 to $650,000, indicating significant salary potential in the
field.

- **Diverse Employers:** Companies like
SmartAsset, Meta, and AT&T are among those offering high salacies, showing a broad interest across different industries.

 - **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.


  ## 2. Skills For Top Payig Jobs

   To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, Providing insights into what employers value for high-compensation roles.

   ```  
WITH  top_paying_jobs AS 
(
SELECT
  job_id,
  job_title,
  salary_year_avg,
  name AS company_name
FROM
   job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
  job_title_short = 'Data Analyst' AND 
  job_location = 'Anywhere'
  and salary_year_avg IS NOT NULL
ORDER BY
   salary_year_avg DESC
LIMIT
   10
)
SELECT
top_paying_jobs. *,
skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC
   ```
 The top most frequently required skills for high-paying Data Analyst role in 2023 are:

- SQL (8 occurrences)

- Python (7 occurrences)
- Tableau (6 occurrences)
- R (4 occurrences)
- Snowflake (3 occurrences)
- Pandas (3 occurrences)
- Excel (3 occurrences)
- Bitbucket (2 occurrences)
- Azure (2 occurrences)
- Power BI (2 occurrences)


## 3. What skills are most in demand for data analysts?

This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```
SELECT 
    skills,
    COUNT(skills_job_dim.skill_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 10;
 ```

## 4. Which skills are associated with higher salaries?

This query explores the average salaries associated with different skills revealed which skills are the highest paying.

```
SELECT 
    skills,
    ROUND (AVG(salary_year_avg), 2) AS avg_salary_yearly
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL AND
    job_work_from_home = 'TRUE'
GROUP BY
    skills
ORDER BY
    avg_salary_yearly DESC
LIMIT 25;
```

## 5. What are the most optimal skills to learn or improve upon?

Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.skill_id) AS demand_count,
    ROUND (AVG(salary_year_avg), 2) AS avg_salary_yearly
FROM  
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND salary_year_avg IS NOT NULL 
    AND job_work_from_home = 'TRUE'
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) >= 20
ORDER BY
    avg_salary_yearly DESC,
    demand_count DESC
LIMIT 25 
```

 #  What I Learned

In this project, I was able to dive deep into SQL, PostgreSQL, and VS Code for the first time, learning both basic and advanced techniques. I was able to apply fundamental concepts of merging tables, using subqueries and CTEs, and using JOIN functions to create complex queries for analysis.

I also learned aggregate functions such as COUNT(), SUM() and AVG() to create insightful data tables, which in turn allowed me to apply my knowledge of Excel spreadsheet to visualize the data for presentation.

Lastly, I learned how to solve real-world problems by using SQL queries to sort complex databases into an actionable solution that is valuable not only to me, but to those who have the same goals and aspirations as me.

 # Conclusion

 **Top-Paying Data Analyst Jobs:**  The highest-paying remote data analyst positions offer a wide salary range, with the top salary reaching an impressive $650,000.

**Skills for Top-Paying Jobs:** Advanced proficiency in SQL is essential for high-paying data analyst roles, underscoring its importance for earning a top salary.

**Most In-Demand Skills:** SQL (along with Python and Excel) is the most demanded skill in the data analyst job market, making it crucial for job seekers to master.

**Skills with Higher Salaries:** Specialized skills, such as SVN and Solidity, are linked to the highest average salaries, highlighting the value of niche expertise.

**Optimal Skills for Job Market Value:** SQL and Python stand out by being in high demand and associated with substantial salaries, making it one of the most valuable skills for data analysts to learn to maximize their market value.

# Closing Thoughts

This project not only honed my SQL skills but also provided valuable insights into the data analyst job market. The findings guide prioritizing skill development and job search strategies. Aspiring data analysts can gain a competitive edge by focusing on high-demand, high-salary skills. This exploration emphasizes the importance of continuous learning and staying updated with emerging trends in data analytics.