# Performance Investigation Report 

This document records a performance troubleshooting investigation, root-cause analysis, optimization applied, and quantitative results.

## 1. Incident Description
- **Target Object:** `SalesLT.Customer`
- **Symptom:** Higher resource consumption and full table traversal when searching for customer records by last name using pattern matching (`LIKE '%...%'`).
- **Initial Metrics (Before):** 
  - Execution Plan Operator: Clustered Index Scan (full table scan).
  - Logical Reads: 37 logical page reads.

## 2. Root Cause Hypothesis
The database engine performed a full table scan because no targeted non-clustered index existed on the `LastName` column to support the filter condition efficiently.

## 3. Implemented Remediation
Created a non-clustered index on the filtering column to improve data retrieval performance:
```sql
CREATE NONCLUSTERED INDEX IX_Customer_LastName 
ON SalesLT.Customer (LastName);
```

## 4. Post-Optimization Results (After)
- **Updated Metrics:**
    - Execution Plan Operator: Optimized index access path.
    - Logical Reads: Reduced to 25 logical page reads.

Improvement Conclusion: The optimization successfully reduced logical I/O and improved query efficiency, validating the index design strategy.

## 5. Limitations & Considerations
While non-clustered indexes accelerate SELECT performance, they introduce minor overhead on write operations (INSERT, UPDATE, DELETE) due to index maintenance. This trade-off must be carefully evaluated in high-throughput transactional environments.