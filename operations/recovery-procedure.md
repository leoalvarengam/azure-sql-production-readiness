# Disaster Recovery & Point-in-Time Restore Architecture 

This document details the automated backup infrastructure, Point-in-Time Restore (PITR) methodology, and disaster recovery procedures for Azure SQL Database.

## 1. Azure SQL Automated Backup Infrastructure

- **Full Backups:** Automatically performed weekly.
- **Differential Backups:** Performed every 12–24 hours.
- **Transaction Log Backups:** Automatically taken every 5 to 10 minutes.
- **Point-in-Time Restore (PITR):** Enables point-in-second granularity restoration within the retention window (default 7 days).

## 2. Theoretical Incident & Recovery Strategy

- **Scenario:** Accidental deletion of critical customer records (`DELETE FROM SalesLT.Customer`).
- **Recovery Plan:**
  1. Identify the exact UTC timestamp prior to the data corruption.
  2. Initiate a Point-in-Time Restore to a dedicated recovery database instance (`AdventureWorksLT_Recovery`).
  3. Validate recovered data integrity isolated from production.
  4. Merge missing records back into the main database or adjust connection strings.

## 3. Environment Limitations & Cost Optimization (FinOps Note)

- **Constraint:** The current lab environment utilizes the Azure SQL Database Free Tier subscription (limited to a single active database instance).
- **Decision:** Physical creation of the secondary restored database (`AdventureWorksLT_Recovery`) was simulated conceptually to avoid non-eligible cloud infrastructure charges.
