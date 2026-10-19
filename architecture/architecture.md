# Environment Architecture

This document describes the initial infrastructure provisioned for the Azure SQL Database administration project.

## Resource Overview
- **Resource Group:** `rg-sql-prod-readiness`
- **Region:** `Brazil South`
- **Logical Server:** `sql-server-prod-readiness.database.windows.net`
- **Database:** `AdventureWorksLT`
- **Service Tier:** Free - General Purpose - Serverless: Gen5, 2 vCores
- **Pricing Model:** vCore

## Connectivity and Initial Security
- **Firewall:** Access restricted to developer IP and enabled for authorized Azure services.
- **Authentication:** SQL Server Authentication

## Selected Service Limits
- **Max Storage:** 32GB
- **DTUs / vCores:** 2 vCores