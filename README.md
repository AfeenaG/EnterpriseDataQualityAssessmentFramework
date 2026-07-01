# Enterprise Data Quality Assessment Framework

> A reusable framework for assessing, profiling, prioritizing, and improving data quality across enterprise datasets.

---

## Overview

This repository documents my structured approach for performing enterprise data quality assessments.

The framework combines SQL profiling, business analysis, root cause analysis, and data governance principles to identify data quality issues, assess business impact, prioritize remediation activities, and recommend long-term preventive controls.

Although this methodology can be applied to any structured dataset, it is particularly suited for operational systems, regulatory reporting, government datasets, and enterprise data warehouses.

---

# Objectives

The objectives of a data quality assessment are to:

- Assess the overall health of a dataset
- Identify data quality issues using recognized quality dimensions
- Quantify the impact of each issue
- Determine the root cause
- Prioritize remediation based on business risk
- Recommend corrective and preventive actions
- Strengthen enterprise data governance

---

# Data Quality Assessment Lifecycle

```text
Understand Business Context
            │
            ▼
Review Data Dictionary
            │
            ▼
Profile the Data
            │
            ▼
Identify Data Quality Issues
            │
            ▼
Quantify Business Impact
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
Validate the Solution
            │
            ▼
Continuous Data Governance
```

---

# Step 1 – Understand the Business Context

Before performing any technical analysis, understand:

- Why does the dataset exist?
- What business processes does it support?
- Who owns the data?
- Who consumes the data?
- What decisions are made using this data?
- What are the downstream systems?

Deliverables

- Business context
- Data owners
- Data stewards
- Business rules
- Reporting requirements

---

# Step 2 – Review Metadata

Review all available metadata.

Examples include

- Data Dictionary
- Entity Relationship Diagram (ERD)
- Reference Tables
- Data Models
- Interface Specifications
- Mapping Documents
- Business Rules

Objective

Understand the expected structure and behavior of the data before profiling begins.

---

# Step 3 – Profile the Dataset

Profile the data using five recognized data quality dimensions.

---

## Completeness

Questions

- Are mandatory fields populated?
- Are required relationships complete?

Typical checks

- NULL values
- Blank values
- Missing foreign keys
- Missing mandatory attributes

Example SQL

```sql
SELECT
SUM(CASE WHEN CustomerName IS NULL THEN 1 ELSE 0 END)
FROM Customer;
```

---

## Accuracy

Questions

- Does the value correctly represent the real-world object?

Typical checks

- Negative currency values
- Impossible dates
- Incorrect calculations
- Invalid measurements

Example SQL

```sql
SELECT *
FROM Orders
WHERE OrderDate > ShipDate;
```

---

## Consistency

Questions

- Does the data agree across related datasets?

Typical checks

- Reference tables
- Cross-field validation
- Parent-child relationships
- Cross-system comparisons

Example SQL

```sql
SELECT *
FROM Orders o
LEFT JOIN StatusReference s
ON o.Status = s.Status
WHERE s.Status IS NULL;
```

---

## Validity

Questions

- Does the data satisfy business rules?

Typical checks

- Enumerated values
- Email validation
- Date ranges
- Numeric ranges
- Mandatory formats

Example SQL

```sql
SELECT *
FROM Employee
WHERE Email NOT LIKE '%@company.com';
```

---

## Uniqueness

Questions

- Should this record exist only once?

Typical checks

- Duplicate primary keys
- Duplicate business keys
- Duplicate transactions

Example SQL

```sql
SELECT CustomerID,
COUNT(*)
FROM Customer
GROUP BY CustomerID
HAVING COUNT(*) > 1;
```

---

# Step 4 – Quantify Findings

For every issue record:

- Number of affected records
- Percentage of total dataset
- Columns affected
- Business process affected
- Severity
- Frequency

Example

| Dimension | Issue | Records | % |
|-----------|---------|---------:|----:|
| Completeness | Missing Units | 15 | 7.5% |
| Accuracy | Negative Value | 8 | 4.0% |

---

# Step 5 – Assess Business Impact

Not every issue should receive the same priority.

Evaluate each issue using:

- Business impact
- Financial impact
- Regulatory impact
- Operational impact
- Customer impact
- Downstream reporting
- System integration
- Decision-making impact

Example

| Issue | Business Impact |
|---------|----------------|
| Missing Housing Units | High |
| Duplicate IDs | High |
| Invalid Email | Low |

---

# Step 6 – Root Cause Analysis

Determine why the issue occurred.

Common causes include

- Missing application validation
- Missing database constraints
- Manual data entry
- Integration failures
- Poor user interface
- Missing reference data
- Missing governance controls

---

# Step 7 – Prioritize Issues

Prioritize based on risk rather than record count.

Evaluation Criteria

- Business Impact
- Operational Risk
- Downstream Use
- Regulatory Risk
- Volume
- Ease of Remediation

Example

| Issue | Priority |
|---------|----------|
| Missing Business Key | High |
| Duplicate Primary Key | High |
| Invalid Email | Low |

---

# Step 8 – Recommend Solutions

Separate recommendations into three categories.

## Immediate Remediation

Examples

- Correct invalid records
- Validate against source systems
- Obtain missing information

---

## Preventive Controls

Examples

- Application validation
- Database constraints
- Reference tables
- Automated validation
- ETL quality checks

---

## Data Governance Improvements

Examples

- Assign Data Steward ownership
- Define business rules
- Establish data quality KPIs
- Implement issue management
- Periodic data quality assessments

---

# Validation

After remediation

- Re-profile the dataset
- Verify corrective actions
- Validate business rules
- Confirm expected results

---

# Deliverables

A completed assessment should produce:

- Data Quality Assessment Report
- SQL Profiling Scripts
- Root Cause Analysis
- Business Impact Assessment
- Prioritization Matrix
- Remediation Plan
- Data Governance Recommendations

---

# Technology Stack

Typical tools include

- SQL Server
- Oracle
- PostgreSQL
- MySQL
- Python
- Pandas
- Power BI
- Excel
- Azure Data Factory
- Microsoft Fabric
- Databricks

---

# Skills Demonstrated

- Data Profiling
- Data Quality Assessment
- Data Governance
- Root Cause Analysis
- SQL
- Python
- Power BI
- Business Analysis
- Data Validation
- Stakeholder Management
- Reporting
- Risk Assessment

---

# Future Enhancements

Planned additions include:

- Automated data profiling in Python
- Great Expectations integration
- Data quality dashboards in Power BI
- Reusable SQL library
- Metadata-driven profiling
- Data quality scorecards
