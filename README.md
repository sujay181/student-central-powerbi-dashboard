# Student Central Dashboard | Power BI

A Power BI dashboard that gives a university's Student Central Head a single view of complaints, club engagement and alumni reach. It is built on a synthetic dataset that I designed.

> **To view the dashboard,** open `Student Central Head.pbix` in Power BI Desktop (see [How to Open](#how-to-open)).

---

## Overview

Southern Texas University (STU) is a fictional university with multiple international campuses. Its Student Central Head is responsible for the student experience: complaints, student clubs and alumni relations. They need one place to see where problems are concentrated and where to act.

The dashboard answers four questions:

1. Where are complaints concentrated, and where is complaint behaviour most widespread?
2. Which departments have gaps in club engagement?
3. Where do alumni live, and which markets are being overlooked?
4. How are satisfaction and complaints changing as intake grows?

---

## My Role

This dashboard was built as part of a three-person group project for a Business Intelligence unit in my Master of Data Science (Professional) at Deakin University. The group built three dashboards, one for each of three stakeholders. **This repository contains only my work:**

- **Dataset design:** I created the synthetic dataset used by the whole group, with 455 student records, 31 attributes and defined business rules.
- **Dashboard build:** I designed and built the Student Central Head dashboard.
- **Analysis:** I wrote the insights and recommendations for Student Central.

The Admission Head and Course Head dashboards were built by my teammates and are not included here.

---

## Dataset

All values are fictional.

- **455 student records** across **31 attributes**, covering demographics, enrolment status, academic performance, engagement, finances, and course and faculty details
- Covers the full student lifecycle: **applicants, enrolled, postponed and alumni**
- Built with **business rules**, including valid ranges, derived fields (such as a grade band derived from WAM and net fee as a calculated field) and lifecycle rules for student status

**Assumptions**

- Applicants who have not yet enrolled have blank or zero academic, engagement and financial values.
- Tuition fees, scholarships and course capacity vary by enrolment year.
- Records are kept for historical and cohort analysis.

<details>
<summary><strong>Full data dictionary (31 attributes)</strong></summary>

| Attribute | Data Type | Description | Business Rule |
| --- | --- | --- | --- |
| Student ID | Text (Alphanumeric) | Unique student identifier | Primary key; immutable |
| Student Name | Text | Full legal name | Cannot be null |
| Course ID | Text (Code) | Unique course code | One-to-one with Course Name |
| Course Name | Text | Full course title | Must match Course ID |
| WAM | Decimal (0–100) | Academic score | Range 0–100 only |
| # Students Referred | Integer (≥ 0) | Number of students referred | Defaults to 0 |
| Satisfaction Score | Integer (1–10) | Satisfaction rating | Rounded integer |
| Total Complaints | Integer (≥ 0) | Number of complaints | Defaults to 0 |
| Engagement Clubs Enrolled | Integer (≥ 0) | Clubs joined | Defaults to 0 |
| Home City | Text | Student's city | Must be a valid city |
| Home Country | Text | Student's country | Dependent on city |
| Grade Band | Text (Category) | Performance band | Derived from WAM |
| Assignment Submission Rate (%) | Decimal (0–100) | Submission percentage | Max 100% |
| Attendance Rate (%) | Decimal (0–100) | Attendance percentage | Max 100% |
| Academic Risk Flag | Text (Category) | Risk level | Derived field |
| Tutoring Sessions Attended | Integer (≥ 0) | Tutoring sessions | Cannot be negative |
| Department | Text (Category) | Academic department | Must be a valid department |
| Course Head | Text | Course leader name | One per course |
| Faculty Teaching Load (Units) | Integer (1–5) | Teaching units assigned | Range 1–5 |
| Course Capacity Utilisation (%) | Decimal (0–100) | Capacity used | Max 100% |
| Delivery Mode | Text (Category) | Course delivery type | Predefined values only |
| Faculty Research Active | Text (Yes/No) | Research status | Yes or No only |
| Scholarship Recipient | Text (Percentage) | Scholarship coverage | 0–100% only |
| Student Status | Text (Category) | Enrolment status | Must follow lifecycle |
| Enrolment Type | Text (Category) | Domestic or International | Two values only |
| Total Course Fee Paid (AUD) | Decimal (AUD) | Total fee charged | ≥ 0 AUD |
| Scholarship Aid Received (AUD) | Decimal (AUD) | Aid received | ≤ total fee |
| Net Fee Paid (AUD) | Decimal (AUD) | Final fee paid | Calculated field |
| Enrolment Year | Integer (Year) | Start year | Not future-dated |
| Retention Risk Score (1–10) | Integer (1–10) | Dropout risk score | Range 1–10 |
| After Graduate Salary (AUD) | Text (Category) | Salary band | Alumni only |

</details>

---

## Dashboard Insights

### 1. Complaint Hotspots by Department

- **Finding:** Computing & IT (254) and Business (231) account for 65% of all 751 complaints. However, Health Sciences has the highest complaint rate at 71%, ahead of Engineering (70%), Computing & IT (66%) and Business (64%).
- **Why it matters:** The number of complaints follows student numbers, but complaint behaviour is most widespread in Health Sciences. Caseload resourcing and a culture review therefore need different targets.

### 2. Club Engagement by Department

- **Finding:** 31% of students belong to no clubs, 40% to one or two, and 29% to three or more. Health Sciences has the largest share of highly engaged students (33%), and Engineering averages 2.12 clubs per student.
- **Why it matters:** Business has the lowest share of highly engaged students (26%) and a 64% complaint rate, which makes it the strongest candidate for a targeted club drive.

### 3. Alumni Footprint by Country

- **Finding:** 94 alumni are spread across 9 countries. The USA (25) and Australia (20) make up 48%, and the remaining 52% are spread across seven other markets. Vietnam (13) has more alumni than China (11) or India (8).
- **Why it matters:** Alumni programmes should put international alumni first. Vietnam is an overlooked outreach market that challenges the usual China and India focus.

### 4. Cohort Trend: Intake, Satisfaction and Complaints

- **Finding:** Intake grew more than five-fold, from 27 students in 2020 to 143 in 2025. The complaint rate stayed between 63% and 72% from 2020 to 2024, then jumped to 87% in 2025, while average satisfaction fell from 7.1 to 5.6.
- **Why it matters:** Student support has not kept pace with intake growth. The 2025 cohort needs stabilising, and support systems need to scale before the 2026 intake.

---

## Recommendations

1. **Stabilise the 2025 cohort.** Provide dedicated case management so that an 87% complaint rate does not become the new baseline.
2. **Run a targeted club drive.** Prioritise Online students, whose club engagement trails Hybrid learners, and the Business department, which has the lowest share of highly engaged students (26%).
3. **Launch an international alumni ambassador programme.** Have it led by 2021 graduates, who are the strongest referrers at 3.4 referrals each, with focused outreach in Vietnam.

Together, these actions link Student Central's three areas of responsibility into one strategy, with 2025 as a measurable baseline.

---

## Key Calculations

| Calculation | What it measures |
| --- | --- |
| Complaint Rate | Share of students who lodged at least one complaint |
| Club Engagement Tier | Groups students into 0 clubs, 1–2 clubs, and 3 or more clubs |
| Average Clubs per Student | Mean number of clubs joined per student |
| Average Satisfaction | Mean satisfaction score (1–10) |
| Alumni Count | Number of students with an Alumni status |
| Average Referrals per Alumnus | Mean number of students referred by alumni |

The full DAX code for these calculations is in [`student_central_measures.md`](student_central_measures.md).

---

## Tools

- **Power BI Desktop:** data modelling, DAX measures and report design
- **Power Query:** data preparation
- **[Excel / Python]:** synthetic dataset creation

---

## Repository Structure

```
student-central-powerbi-dashboard/
├── README.md
├── Student Central Head.pbix
├── StudentCentral_PowerBI_Dataset.xlsx
└── student_central_measures.md
```

---

## How to Open

1. Download or clone this repository.
2. Open `Student Central Head.pbix` in [Power BI Desktop](https://www.microsoft.com/power-platform/products/power-bi/desktop), which is free for Windows.
3. The data is already loaded, so the report works as it is. To refresh it, go to **Transform data → Data source settings → Change Source** and point it to `StudentCentral_PowerBI_Dataset.xlsx` in your downloaded copy.

---

## Author

**Sujay Narayana**
[LinkedIn](https://www.linkedin.com/in/sujay-narayana-646b17213/) · [GitHub](https://github.com/sujay181)
