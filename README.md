# Enterprise Data Quality Assessment Framework

> A reusable framework for profiling, assessing, prioritizing, and improving enterprise data quality using SQL, Python, and modern data governance principles.

---

## Overview

This repository documents my structured approach to performing enterprise data quality assessments.

The framework combines data profiling, business analysis, root cause analysis, and data governance to identify data quality issues, assess business impact, prioritize remediation efforts, and recommend long-term preventive controls.

Although the examples throughout this repository are illustrative, the methodology can be applied across operational databases, data warehouses, analytics platforms, regulatory reporting systems, and enterprise applications.

---

> **Disclaimer**

This repository demonstrates a reusable enterprise data quality assessment framework developed for professional and educational purposes.

All examples, SQL templates, visualizations, and metrics are illustrative and are intended to demonstrate methodology only. They do not represent any production, proprietary, confidential, or client-specific datasets.

---

# Objectives

The objective of a data quality assessment is to:

- Assess the overall health of a dataset
- Identify data quality issues using recognized quality dimensions
- Quantify the impact of each issue
- Determine the underlying root cause
- Prioritize remediation based on business risk
- Recommend corrective and preventive controls
- Strengthen enterprise data governance

---

# Data Quality Assessment Lifecycle

```text
Understand Business Context
            │
            ▼
Review Metadata & Business Rules
            │
            ▼
Profile the Dataset
            │
            ▼
Identify Data Quality Issues
            │
            ▼
Quantify Findings
            │
            ▼
Assess Business Impact
            │
            ▼
Root Cause Analysis
            │
            ▼
Prioritize Issues
            │
            ▼
Recommend Corrective Actions
            │
            ▼
Implement Preventive Controls
            │
            ▼
Validate Implementation
            │
            ▼
Continuous Data Governance
```

---

# Step 1 – Understand the Business Context

Before writing SQL or profiling data, understand the business.

Questions to answer include:

- What is the purpose of the dataset?
- Which business processes depend on it?
- Who owns the data?
- Who consumes the data?
- What decisions are made using the data?
- Which downstream systems depend on the data?

Deliverables:

- Business Context
- Data Owners
- Data Stewards
- Business Rules
- Reporting Requirements
- Critical Data Elements

---

# Step 2 – Review Metadata

Review all available metadata before beginning analysis.

Typical metadata includes:

- Data Dictionary
- Entity Relationship Diagram (ERD)
- Data Models
- Reference Tables
- Interface Specifications
- Mapping Documents
- Business Rules

The objective is to understand the expected structure and behavior of the data before assessing its quality.

---

# Step 3 – Profile the Dataset

Evaluate the dataset using five recognized data quality dimensions.

---

## 1. Completeness

**Question**

> Is all required information present?

Typical checks:

- NULL values
- Blank values
- Missing mandatory attributes
- Missing foreign keys

Illustrative SQL

```sql
SELECT
SUM(CASE WHEN CustomerName IS NULL THEN 1 ELSE 0 END)
FROM Customer;
```

---

## 2. Accuracy

**Question**

> Does the data correctly represent the real-world entity?

Typical checks:

- Negative monetary values
- Impossible dates
- Invalid calculations
- Out-of-range numeric values

Illustrative SQL

```sql
SELECT *
FROM Orders
WHERE OrderDate > ShipDate;
```

---

## 3. Consistency

**Question**

> Does the data remain consistent across related datasets and business rules?

Typical checks:

- Reference table validation
- Cross-field validation
- Parent-child relationships
- Cross-system reconciliation

Illustrative SQL

```sql
SELECT *
FROM Orders o
LEFT JOIN StatusReference s
ON o.Status = s.Status
WHERE s.Status IS NULL;
```

---

## 4. Validity

**Question**

> Does the data conform to defined business rules?

Typical checks:

- Enumerated values
- Email formats
- Numeric ranges
- Date ranges
- Pattern validation

Illustrative SQL

```sql
SELECT *
FROM Employee
WHERE Email NOT LIKE '%@company.com';
```

---

## 5. Uniqueness

**Question**

> Should each record exist only once?

Typical checks:

- Duplicate primary keys- Duplicate business keys
- Duplicate transactions

Illustrative SQL

```sql
SELECT CustomerID,
COUNT(*)
FROM Customer
GROUP BY CustomerID
HAVING COUNT(*) > 1;
```

---

# Step 4 – Quantify Findings

Every issue should be quantified.

Typical metrics include:

- Number of affected records
- Percentage of dataset affected
- Columns impacted
- Business process impacted
- Severity
- Frequency

Illustrative Example

| Dimension | Rule | Records | Health % |
|------------|----------------------------|--------:|-----------:|
| Completeness | Mandatory Field Missing |15 | 92.5 | % |
| Accuracy | Invalid Date Sequence | 21 | 89.5% |
| Consistency | Reference Data Mismatch | 38 | 81% |
| Validity | Invalid Format | 50 | 75% |
| Uniqueness | Duplicate Business Key | 6 | 97% |

![Data Quality Profiling Results](/images/DataQualityDimensions.png)

---

# Step 5 – Assess Business Impact

Not every issue requires immediate remediation.

Each issue should be evaluated using a risk-based approach.

Consider:

- Business impact
- Operational impact
- Financial impact
- Regulatory impact
- Customer impact
- Downstream reporting
- System integration
- Decision-making impact

Example

| Issue | Business Impact |
|-------------------------------|---------|
| Missing Mandatory Attribute | High |
| Duplicate Business Key | High |
| Invalid Email Format | Medium |
| Reference Data Mismatch | Medium |

---

# Step 6 – Root Cause Analysis

Correcting data without understanding why the issue occurred often leads to recurring problems.

Common root causes include:

- Missing application validation
- Missing database constraints
- Manual data entry
- Poor user interface design
- Missing reference data
- Integration failures
- Legacy system limitations
- Ambiguous business rules

The objective is to implement preventive controls rather than repeatedly correcting the same issues.

---

# Step 7 – Prioritize Issues

Prioritize remediation based on organizational risk—not simply the number of affected records.

Evaluation criteria include:

- Business impact
- Operational risk
- Regulatory compliance
- Financial impact
- Downstream dependencies
- Volume of affected records
- Root cause complexity
- Ease of remediation

Illustrative Priority Matrix

| Priority | Description |
|-----------|-------------|
| High | Significant business or regulatory impact requiring immediate action |
| Medium | Moderate impact with manageable operational risk |
| Low | Limited impact; remediation can be scheduled |

---

# Step 8 – Recommend Corrective Actions

Recommendations should be grouped into three categories.

---

## Immediate Remediation

Examples:

- Correct invalid records
- Validate against source systems
- Obtain missing information
- Update historical records

---

## Preventive Controls

Examples:

- Application validation
- Database constraints
- Reference table validation
- Automated quality checks
- ETL validation rules

---

## Data Governance Improvements

Examples:

- Assign Data Steward ownership
- Maintain business rules
- Maintain reference data
- Monitor Data Quality KPIs
- Track remediation activities
- Conduct periodic assessments
- Establish continuous improvement processes

---

# Step 9 – Validate the Solution

After implementing corrective actions:

- Re-profile the dataset
- Verify that issues have been resolved
- Confirm business rules are enforced
- Validate expected outcomes with stakeholders
- Ensure no new issues have been introduced

---

# Deliverables

A completed assessment typically produces:

- Data Quality Assessment Report
- SQL Profiling Scripts
- Root Cause Analysis
- Business Impact Assessment
- Prioritization Matrix
- Remediation Plan
- Data Governance Recommendations
- Executive Dashboard

---

# Technology Stack

Typical technologies include:

### Databases

- SQL Server
- Oracle
- PostgreSQL
- MySQL

### Programming

- Python
- Pandas
- NumPy

### Analytics & Visualization

- Power BI
- Excel

### Cloud & Modern Data Platforms

- Microsoft Fabric
- Azure Data Factory
- Azure Databricks

---

# Skills Demonstrated

- Data Profiling
- Data Quality Assessment
- Data Governance
- SQL
- Python
- Root Cause Analysis
- Business Analysis
- Risk Assessment
- Data Validation
- Data Stewardship
- Stakeholder Communication
- Reporting & Dashboarding

---

# Repository Structure

```text
enterprise-data-quality-framework/
│
├── README.md
├── sql/
│   ├── completeness.sql
│   ├── accuracy.sql
│   ├── consistency.sql
│   ├── validity.sql
│   └── uniqueness.sql
│
├── python/
│   └── data_profiling.ipynb
│
├── powerbi/
│   └── Data_Quality_Dashboard.pbix
│
├── docs/
│   ├── methodology.md
│   ├── prioritization-framework.md
│   └── root-cause-analysis.md
│
└── images/
```

---

# Future Enhancements

Planned enhancements include:

- Automated SQL profiling library
- Metadata-driven rule generation
- Python data profiling package
- Great Expectations integration
- Microsoft Fabric implementation
- Data Quality Scorecards
- Automated Power BI dashboards
- Enterprise Data Quality KPI monitoring

---

# License

This repository is intended for educational and professional portfolio purposes. The framework may be adapted and extended to support enterprise data quality initiatives across different industries and technology platforms.
