# Operational Monitoring & Investigation Guide 

This document defines the key performance indicators (KPIs), diagnostic techniques, alerting strategies, and troubleshooting workflows for Azure SQL Database.

## 1. Core Performance Metrics (Azure Monitor)
- **CPU Percentage (`cpu_percent`):** Monitors compute capacity. Spikes (>80%) indicate unoptimized queries or high execution frequency.
- **Data IO Percentage:** Tracks physical disk read operations. High values point to missing indexes forcing full table scans.
- **Log IO Percentage:** Measures write activity in transaction logs during heavy data modifications.
- **Connection Count:** Monitors active and concurrent user connections to detect connection leaks.

## 2. In-Database Diagnostics via Query Store
Query Store is enabled natively to track execution plan changes and resource-intensive queries. Top resource-consuming queries are identified using catalog views (`sys.query_store_runtime_stats`).

## 3. Troubleshooting Workflow for Resource Spikes
1. **Identify the Symptom:** Check Azure Portal **Metrics** to determine if the bottleneck is CPU, Data IO, or Log IO.
2. **Isolate Top Queries:** Query `sys.query_store_query` or use Azure Portal **Query Performance Insight** to pinpoint the exact T-SQL text causing the spike.
3. **Analyze Execution Plans:** Inspect missing index recommendations or parameter sniffing issues.
4. **Apply Remediation:** Create supporting indexes, tune SQL syntax, or scale up service tier resources if workload demands exceed current tier limits.

## 4. Alerting & Cost Considerations (FinOps)
- **Metric Alerts:** Configured to trigger email/webhooks when `CPU Percentage` exceeds 85% for 5 consecutive minutes.
- **Cost Implications:**
  - Standard Azure Metrics & Query Store usage are free of charge.
  - Exporting Diagnostic Logs to a **Log Analytics Workspace** incurs charges based on data ingestion volume (GBs/month) and retention policies.