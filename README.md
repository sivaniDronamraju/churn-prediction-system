# Customer Churn Risk Modeling

## Overview
Customer churn directly impacts revenue, customer lifetime value, and long-term growth.  
This project implements an end-to-end churn risk modeling system that combines data analysis, machine learning, and cloud infrastructure to help organizations identify customers at risk of leaving and surface insights that support proactive retention decisions.

The system focuses on producing **decision-ready outputs** rather than model accuracy alone. Predictions are stored, visualized, and optionally surfaced through alerts so that business and analytics teams can act on them without needing to interact with raw model artifacts.

## Problem Context
Most customer churn problems are not purely predictive, they are operational.  
Teams need to understand:
- **Who** is at risk of leaving
- **Why** certain customers churn more than others
- **How** churn risk changes across customer segments
- **Where** to intervene without overwhelming stakeholders with noise

This project is framed around the **customer lifecycle**, with churn risk treated as an input to downstream analysis and decision-making rather than an isolated modeling task.

## Data & Scope
The analysis uses a structured customer dataset containing demographic, financial, and engagement attributes for approximately 10,000 customers. The data represents a simplified banking-style customer lifecycle and includes signals such as account balance, tenure, product usage, activity status, and churn outcome.

The dataset is used to:
- Explore behavioral patterns associated with churn
- Train and evaluate classification models under class imbalance
- Generate predictions that can be consumed by dashboards and alerting workflows


## System Architecture
The system is implemented using AWS-managed services to support scalability, separation of concerns, and clean hand-offs between stages.

At a high level:
- **Amazon S3** is used as the system of record for raw data, processed features, and model outputs
- **Amazon SageMaker** is used for data preprocessing, feature transformation, model training, and evaluation
- **Amazon QuickSight** consumes model outputs to support interactive analysis and stakeholder-friendly dashboards
- **Event-based notifications** (SNS + Lambda + SES) are optionally used to surface high-level churn risk updates when model outputs change meaningfully

The architecture is intentionally kept simple, with cloud services used to reduce operational overhead rather than introduce unnecessary complexity.


## Modeling Approach
Customer churn is treated as a binary classification problem with a **moderately imbalanced target**, where false negatives (missing at-risk customers) and false positives (unnecessary interventions) both carry cost.

Key aspects of the modeling approach:
- Multiple baseline and tree-based models were evaluated, including Logistic Regression, SVM, Decision Trees, KNN, and Random Forests
- **F1-score** was used as the primary evaluation metric to balance precision and recall under class imbalance
- Stratified train / validation / test splits were used to maintain label balance across datasets
- Feature transformations and normalization were applied where appropriate to support stable model behavior

A Random Forest model provided the strongest overall balance between performance and interpretability on the validation set and was used to generate final predictions.


## Key Insights
Exploratory analysis and model outputs highlighted several consistent patterns:

- Customers with **higher account balances** tend to churn less, suggesting stronger financial engagement
- **Inactive members** show significantly higher churn rates than active users
- **Older customers** are more likely to churn, while middle-aged customers tend to hold the highest balances
- Customers with **fewer products** exhibit higher churn risk, indicating potential cross-sell or engagement opportunities
- Churn likelihood varies meaningfully across balance tiers, making segmentation more actionable than global thresholds

These insights are surfaced through dashboards rather than static reports to support ongoing exploration.


## Operational Outputs
Rather than stopping at model evaluation, the system produces outputs designed for consumption:

- **Prediction datasets** stored in S3 for downstream analysis
- **Interactive dashboards** that allow stakeholders to explore churn risk across customer segments
- **Optional alerting logic** that notifies stakeholders only when aggregate churn risk or key metrics change beyond predefined thresholds, avoiding per-customer noise


