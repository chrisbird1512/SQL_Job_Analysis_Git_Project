
---

# 📊 Data Analyst Job Market Analysis (SQL Project)

> An in-depth SQL analysis of Data Analyst job postings to uncover top-paying roles, in-demand skills, and the most valuable skills based on salary and demand.

---

## 📌 Project Overview

This project analyzes real-world job posting data to answer key career-focused questions:

* What are the highest-paying Data Analyst jobs?
* What skills are required for those top-paying roles?
* What skills are most in demand?
* Which skills are associated with the highest salaries?
* What are the optimal skills based on both demand and compensation?

The analysis uses advanced SQL techniques including CTEs, joins, aggregations, filtering, and sorting.

---

## 🗂️ Database Schema

The project uses four relational tables:

| Table               | Description                                         |
| ------------------- | --------------------------------------------------- |
| `job_postings_fact` | Job posting details (title, salary, location, etc.) |
| `company_dim`       | Company metadata                                    |
| `skills_job_dim`    | Bridge table linking jobs and skills                |
| `skills_dim`        | Skills reference table                              |

---

## 🛠️ SQL Skills Demonstrated

* INNER JOIN / LEFT JOIN
* Common Table Expressions (CTEs)
* GROUP BY & Aggregations
* COUNT() and AVG()
* Filtering with WHERE
* Sorting with ORDER BY
* Data cleaning using `IS NOT NULL`
* Multi-condition ranking logic

---

# 📈 Analysis & Queries

---

## 1️⃣ Top 25 Highest-Paying Data Analyst Jobs

**Goal:** Identify the highest-paying Data Analyst roles in:

* Remote ("Anywhere")
* Denver, CO

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM job_postings_fact
LEFT JOIN company_dim 
    ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location IN ('Anywhere', 'Denver, CO')
    AND salary_year_avg IS NOT NULL
    AND job_schedule_type IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 25;
```

### 🔎 Insight

Reveals compensation benchmarks and companies offering premium salaries.

---

## 2️⃣ Skills Required for Top-Paying Jobs

**Goal:** Identify skills associated with the top 25 highest-paying roles.

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM job_postings_fact
    LEFT JOIN company_dim 
        ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst'
        AND job_location IN ('Anywhere', 'Denver, CO')
        AND salary_year_avg IS NOT NULL
        AND job_schedule_type IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 25
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim 
    ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```

### 🔎 Insight

Highlights high-value technical skills tied directly to top compensation.

---

## 3️⃣ Most In-Demand Skills

**Goal:** Determine which skills appear most frequently in Data Analyst job postings.

```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 25;
```

### 🔎 Insight

Identifies foundational skills necessary to stay competitive in the job market.

---

## 4️⃣ Highest-Paying Skills

**Goal:** Find which skills are associated with the highest average salaries.

```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
GROUP BY skills
ORDER BY avg_salary DESC
LIMIT 25;
```

### 🔎 Insight

Reveals which technical skills command premium compensation.

---

## 5️⃣ Optimal Skills: High Demand + High Salary

**Goal:** Identify the most strategically valuable skills based on both demand and salary.

```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
GROUP BY skills_dim.skill_id
ORDER BY 
    demand_count DESC,
    avg_salary DESC
LIMIT 25;
```

### 🔎 Insight

Pinpoints the best ROI skills for career growth — high employer demand + high salary return.

---

# 📊 Key Findings (Example Themes)

* SQL consistently appears among the most in-demand skills.
* Advanced tools (e.g., Python, BI tools, cloud platforms) correlate with higher salaries.
* Remote roles often offer competitive compensation.
* Certain niche technical skills show high salary potential despite lower demand.

---

# 🎯 Business & Career Value

This project demonstrates the ability to:

* Translate business questions into SQL queries
* Perform multi-table relational joins
* Use data aggregation for decision-making
* Extract actionable insights from raw job market data
* Apply analytical thinking to real-world datasets


---

# 🧠 Author

**Christopher Bird**
  -  Data Analyst | SQL | Data Visualization | Business Intelligence

---

