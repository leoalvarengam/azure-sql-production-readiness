# Security Review & Access Control Matrix

This document outlines the security architecture, authentication methods, and the implementation of the Principle of Least Privilege (PoLP) for the Azure SQL database environment.

## 1. Authentication Mechanisms
- **Azure SQL Authentication:** Used for database-level user management via server logins and contained database users.
- **Auditing & Security Baseline:** Network access is restricted via server-level firewall rules.

## 2. Role-Based Access Control (RBAC) / Permission Matrix

| Database User | Target Schema / Tables | Allowed Operations (`GRANT`) | Denied / Restricted Operations (`DENY`) | Business Justification |
| :--- | :--- | :--- | :--- | :--- |
| `app_backend_user` | `SalesLT.Customer`, `SalesOrderHeader`, `SalesOrderDetail`, `Product` | `SELECT`, `INSERT`, `UPDATE` | `DELETE` on Order tables | Backend application needs to read catalog and process orders, but must never purge historical order data. |
| `data_analyst_user` | `SalesLT.Customer`, `SalesOrderHeader`, `SalesOrderDetail`, `Product` | `SELECT` | `INSERT`, `UPDATE`, `DELETE` | BI / Analytics tools require read-only reporting access without risk of modifying production records. |

## 3. Security Verification Evidence
- Verification scripts and execution error screenshots are stored under `evidence/screenshots/` demonstrating that unauthorized actions (e.g., `DELETE` by app user, `INSERT` by analyst user) are successfully rejected by the database engine.