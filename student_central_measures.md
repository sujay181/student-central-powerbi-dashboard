# DAX Measures – Student Central Dashboard

All calculations are built on a single table, `Students`, loaded from `StudentCentral_PowerBI_Dataset.xlsx`. They are grouped by the dashboard visual they support.

Measures ending in *Rate*, *Share* or *%* are formatted as percentages in Power BI.

---

## Base Measures

### Total Students
Counts unique students in the current filter context. It is the denominator for most rates.

```dax
Total Students =
DISTINCTCOUNT ( Students[Student ID] )
```

### Complaint Count
Total number of complaints lodged.

```dax
Complaint Count =
SUM ( Students[Total Complaints] )
```

### Average Satisfaction
Average satisfaction score on the 1–10 scale. Applicants who have no score yet are excluded, so they don't pull the average down.

```dax
Average Satisfaction =
CALCULATE (
    AVERAGE ( Students[Satisfaction Score] ),
    Students[Satisfaction Score] > 0
)
```

---

## Chart 1: Complaint Hotspots by Department

### Students with Complaints
Number of students who lodged at least one complaint.

```dax
Students with Complaints =
CALCULATE (
    [Total Students],
    Students[Total Complaints] > 0
)
```

### Complaint Rate
Share of students who lodged at least one complaint. It shows how widespread complaint behaviour is, regardless of department size.

```dax
Complaint Rate =
DIVIDE ( [Students with Complaints], [Total Students] )
```

### % of All Complaints
Each department's share of all complaints. The department filter is removed from the denominator, so the shares add up to 100%.

```dax
% of All Complaints =
DIVIDE (
    [Complaint Count],
    CALCULATE ( [Complaint Count], REMOVEFILTERS ( Students[Department] ) )
)
```

---

## Chart 2: Club Engagement by Department

### Club Engagement Tier *(calculated column)*
Groups students by the number of clubs joined. Blank values are treated as 0.

```dax
Club Engagement Tier =
SWITCH (
    TRUE (),
    Students[Engagement Clubs Enrolled] = 0, "None (0)",
    Students[Engagement Clubs Enrolled] <= 2, "Moderate (1–2)",
    "High (3+)"
)
```

### Club Tier Order *(calculated column)*
Sort key that makes the tiers display as None → Moderate → High instead of alphabetically. It is derived from the source column rather than from the tier label, which avoids a circular dependency when it's used with **Sort by column**.

```dax
Club Tier Order =
SWITCH (
    TRUE (),
    Students[Engagement Clubs Enrolled] = 0, 1,
    Students[Engagement Clubs Enrolled] <= 2, 2,
    3
)
```

### Average Clubs per Student
Mean number of clubs joined per student.

```dax
Average Clubs per Student =
AVERAGE ( Students[Engagement Clubs Enrolled] )
```

### % of Students
Share of students in each tier. With Department on the axis and the tier in the legend, each department's bar adds up to 100%. Both the tier column and its sort column are cleared from the denominator, because Power BI groups visuals by the sort column as well.

```dax
% of Students =
DIVIDE (
    [Total Students],
    CALCULATE (
        [Total Students],
        REMOVEFILTERS ( Students[Club Engagement Tier], Students[Club Tier Order] )
    )
)
```

### High Engagement %
Share of students who belong to three or more clubs.

```dax
High Engagement % =
DIVIDE (
    CALCULATE ( [Total Students], Students[Engagement Clubs Enrolled] >= 3 ),
    [Total Students]
)
```

---

## Chart 3: Alumni Footprint by Country

### Alumni Count
Number of students with an Alumni status.

```dax
Alumni Count =
CALCULATE (
    [Total Students],
    Students[Student Status] = "Alumni"
)
```

### Alumni Share
Each country's share of all alumni.

```dax
Alumni Share =
DIVIDE (
    [Alumni Count],
    CALCULATE ( [Alumni Count], REMOVEFILTERS ( Students[Home Country] ) )
)
```

### Average Referrals per Alumnus
Average number of students referred by each graduate. It identifies which graduating cohorts make the strongest ambassadors.

```dax
Average Referrals per Alumnus =
CALCULATE (
    AVERAGE ( Students[# Students Referred] ),
    Students[Student Status] = "Alumni"
)
```

---

## Chart 4: Cohort Trend

Intake by year uses **Total Students** with Enrolment Year on the axis. Satisfaction and complaint trends use **Average Satisfaction** and **Complaint Rate**.

### Complaint Rate Change
Change in complaint rate compared with the previous enrolment year, in percentage points. It highlights the 2025 jump from 72% to 87%.

```dax
Complaint Rate Change =
VAR CurrentYear = SELECTEDVALUE ( Students[Enrolment Year] )
VAR PriorRate =
    CALCULATE (
        [Complaint Rate],
        Students[Enrolment Year] = CurrentYear - 1
    )
RETURN
    IF ( NOT ISBLANK ( PriorRate ), [Complaint Rate] - PriorRate )
```
