# Global Data Jobs Analysis 2020–2026

**Author:** AbdulKabeer Mabinuori  
**Tool:** Microsoft Excel (Power Pivot + DAX)  
**Dataset:** 10,345 job postings across 7 countries | 2020–2026  

---

## Overview

This project analyses the global data jobs market across 7 countries from 2020 to 2026. The goal was to uncover actionable insights for data professionals, answering questions around salary benchmarks, in-demand skills, hiring trends, and role targeting.

The interactive Excel dashboard built with Power Pivot, DAX measures, and dynamic slicers, designed to tell a clear, data-driven story to both technical and non-technical audiences.

---

## Business Questions Answered

1. How large is the global data jobs market and is it growing?
2. What is the average salary for data professionals and how has it changed year over year?
3. Which role commands the highest salary?
4. Which skills deliver the greatest salary premium?
5. Which countries and industries have the highest demand?
6. How does experience level impact compensation?
7. Does remote, hybrid, or onsite work affect salary?

---

## Dataset

| Field | Description |
|---|---|
| `job_id` | Unique identifier for each posting |
| `job_title` | Role title (Data Analyst, AI Engineer, etc.) |
| `company_size` | Startup, MNC, Enterprise, Medium |
| `company_industry` | Technology, Finance, Healthcare, etc. |
| `country` | 7 countries: USA, UK, Canada, Germany, India, Singapore, Australia |
| `remote_type` | Remote, Hybrid, Onsite |
| `experience_level` | Entry, Mid, Senior |
| `years_experience` | Years of experience required |
| `education_level` | Required education level |
| `skills_python` | Binary flag — Python required (1/0) |
| `skills_sql` | Binary flag — SQL required (1/0) |
| `skills_ml` | Binary flag — ML required (1/0) |
| `skills_deep_learning` | Binary flag — Deep Learning required (1/0) |
| `skills_cloud` | Binary flag — Cloud required (1/0) |
| `salary` | Annual salary in USD |
| `job_posting_month` | Month of posting |
| `job_posting_year` | Year of posting (2020–2026) |
| `hiring_urgency` | Low / High |
| `job_openings` | Number of openings per posting |

---

## Process

### Step 1 — Data Cleaning
Validated the dataset across all key columns before analysis:

- Checked for missing values across all 19 columns → **0 blanks found**
- Checked for duplicate rows on `job_id` → **0 duplicates found**
- Validated salary range — no zero or negative values
- Validated year range — all records within 2020–2026
- Confirmed category consistency (no mixed casing in `remote_type`, `experience_level`, `hiring_urgency`)

**Result:** Dataset was clean and required no transformation before loading into Power Pivot.

### Step 2 — Exploratory Data Analysis (EDA)
Explored distributions, central tendency, and relationships across key variables:

**Salary Distribution:**
- Min: $45,083 | Max: $204,143 | Range: $159,060
- Mean: $113,438 | Median: $113,082
- Mean and median are nearly identical — symmetric distribution, no significant outliers

**Job Openings Distribution:**
- Min: 1 | Max: 9 | Mean: 5 | Median: 5
- Near-uniform distribution across all posting sizes

**Key patterns identified:**
- Two distinct salary tiers: AI Engineer & ML Engineer (~$140K) vs all other roles (~$99K–$102K)
- Salary trend is remarkably flat across 2020–2026 — less than 1% variation year over year
- ML and Deep Learning skills command the largest salary premiums
- Experience level is a stronger salary driver than country or remote type

### Step 3 — Analysis
Built summary tables in Power Pivot to feed the dashboard:

- Average salary by job title, experience level, industry, and company size
- Total job openings by country and company size
- Skills premium analysis — average salary with vs without each skill
- Year-over-year salary and job openings change (2020–2026)
- Work arrangement split (Remote / Hybrid / Onsite)

### Step 4 — Dashboard
Built an interactive dashboard in Excel using Power Pivot and DAX with:

- 4 KPI cards with dynamic YoY indicators
- 9 charts covering salary, demand, skills, and experience
- Year slicer connected to all visuals
- Job title and remote type slicers for drill-down

## Dashboard
![Dashboard](https://github.com/mabinuoriabdulkabeer/Global-Data-Jobs-Analysis-2020-2026/blob/main/Global%20Data%20Jobs%20Analysis%20Dashboard.png)

---

## Key Insights

**1. Two salary tiers exist in the data market**
AI Engineer ($139,945) and Machine Learning Engineer ($139,705) earn ~$40,000 more than the four remaining roles which cluster tightly between $99K–$102K.

**2. Deep Learning is the most valuable skill**
Professionals with Deep Learning skills earn $121,080 on average vs $105,857 without — a premium of **+$15,223**. ML follows closely at **+$14,613**.

**3. Python and SQL add minimal salary lift**
Despite being the most common requirements, Python adds only $599 and SQL actually shows a slight negative correlation (-$218). These are table stakes — not differentiators.

**4. Salary has been stable for 6 years**
Average salary has remained within a 1% band from 2020 to 2026, suggesting the data jobs market has reached compensation equilibrium rather than showing growth or decline.

**5. Experience drives salary more than location**
Entry-level: $89,096 | Mid-level: $113,592 | Senior: $138,289. The $49,193 gap from Entry to Senior is the strongest salary driver in the dataset — stronger than country, industry, or remote type.

**6. Germany leads job demand, but all markets are competitive**
Germany (7,530 openings) leads slightly, but the range across all 7 countries is only 291 postings — the market is globally distributed with no dominant single country.

---

## DAX Measures

```dax
-- Base measures
Average Salary = AVERAGE('Dataset'[salary])
Total Job Openings = SUM('Dataset'[job_openings])

-- YoY KPI text measures (with conditional formatting ▲▼)
Avg Salary YoY KPI =
VAR Curr = [Average Salary]
VAR CurrentYear = MAX('Dataset'[job_posting_year])
VAR MultiYear = DISTINCTCOUNT('Dataset'[job_posting_year]) > 1
VAR PrevYear = CurrentYear - 1
VAR Prev =
    CALCULATE(
        [Average Salary],
        ALL('Dataset'),
        'Dataset'[job_posting_year] = PrevYear
    )
VAR ChangePct = DIVIDE(Curr - Prev, Prev)
RETURN
IF(MultiYear, "Select Year",
    IF(ISBLANK(Prev) || Prev = 0, "No Prior Year",
        IF(ChangePct > 0, "vs " & PrevYear & " ▲ " & FORMAT(ChangePct, "0.0%"),
            IF(ChangePct < 0, "vs " & PrevYear & " ▼ " & FORMAT(ABS(ChangePct), "0.0%"),
                "vs " & PrevYear & " ▶ 0%"))))
```

---

## Dashboard Features

| Feature | Detail |
|---|---|
| KPI Cards | Total Job Openings, Avg Salary, Top Paying Role, Skills Premium |
| YoY Indicators | Dynamic ▲▼ arrows with conditional green/red formatting |
| Charts | Salary trend, demand by country, salary by industry, salary by job title, work arrangement split, salary by experience, salary by company size, job postings by company size, skills premium comparison |
| Slicers | Year (2020–2026), Job Title, Remote Type |
| Navigation | Sheet navigation buttons (Dashboard / Dataset / Pivot Table) |

---

## Tools & Techniques

- Microsoft Excel — Power Pivot, DAX, PivotTables, Slicers
- DAX measures — `CALCULATE`, `ALL`, `DIVIDE`, `DISTINCTCOUNT`, `FORMAT`
- Camera Tool — live-linked KPI snapshots on dashboard sheet
- Conditional Formatting — formula-based ▲▼ colour rules on DAX text output
- Chart types — Line, Bar, Column, Donut, Sparklines

---

## Author

**AbdulKabeer Mabinuori**  
Data Analyst  

[(https://www.linkedin.com/in/abdulkabeer-mabinuori-144068386)]  
