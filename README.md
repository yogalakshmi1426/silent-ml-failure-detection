
# Silent ML Failure Detection System

## Project Overview
Machine learning models often fail silently in production due to changes in incoming data rather than code errors. These failures are difficult to detect because the system continues to produce predictions, even though model performance may degrade significantly.

This project implements a simple MLOps-style monitoring system that detects silent ML failures by comparing new incoming data against a baseline data profile.

---

## Problem Statement
In real-world ML systems, data distributions can change over time due to sensor issues, data pipeline failures, or real-world behaviour changes. These changes can cause models to produce unreliable predictions without triggering obvious system errors.

The goal of this project is to detect such silent failures early by monitoring:
- Schema changes
- Missing value spikes
- Statistical distribution drift

---

## Dataset
The project uses a real-world air quality dataset containing environmental sensor readings from multiple cities. Each city is provided as a separate CSV file, simulating multi-source data ingestion commonly seen in production ML systems.

This dataset is suitable for monitoring because environmental data naturally exhibits drift, missing values, and variability across sources.

---

## System Design
The system follows a simple end-to-end monitoring pipeline:

1. Ingest multiple CSV files from different data sources
2. Combine them into a unified dataset with source tracking
3. Create a baseline data profile representing normal behaviour
4. Store the baseline profile as a reusable JSON artifact
5. Compare new incoming data against the baseline
6. Detect silent failures and assign severity levels

---

## Baseline Profiling
A baseline profile is created using historical data to capture normal data behaviour. The profiling step records:

- Dataset schema (column names and data types)
- Missing value percentages per column
- Statistical summaries of numeric features
- Categorical value distributions

The baseline profile is stored as a JSON file and serves as a reference point for future monitoring.

---

## Drift & Silent Failure Detection
When new data arrives, the system profiles it using the same logic and compares it against the baseline profile to detect:

- **Schema drift**: missing or newly introduced columns
- **Missing value drift**: sudden increases in missing data
- **Statistical drift**: significant shifts in numeric feature means

These checks help identify situations where an ML model may no longer be operating under expected data conditions.

---

## Failure Scenarios Simulated
To demonstrate the monitoring system, the following real-world failure scenarios are intentionally simulated:

- Sudden increase in missing sensor values
- Statistical drift caused by abnormal changes in pollution levels
- Distribution shifts that would silently degrade model performance

---

## Severity & Decision Logic
Detected issues are classified into severity levels:

- **INFO**: Minor changes that do not require immediate action
- **WARNING**: Potential risk to model reliability
- **CRITICAL**: High-risk issues such as schema changes that may require stopping predictions or retraining models

This mirrors real-world decision-making in production ML systems.

---

## How This Would Work in Production
In a production environment, this system could run as a scheduled monitoring job (e.g., daily or hourly). The baseline profile would be stored as a persistent artifact, and alerts could be sent to the ML team when drift thresholds are exceeded.

The system can be extended to integrate with cloud services such as AWS S3 for storage and CloudWatch for monitoring and alerting.

---

## Future Improvements
Possible extensions to this project include:
- Automated retraining triggers
- Time-window-based drift detection
- Integration with dashboards for visualization
- Support for model performance monitoring alongside data drift
