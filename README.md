# 📊 IBM Watson Analytics: Customer Churn Prediction

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![Library](https://img.shields.io/badge/Library-Scikit--Learn%20%7C%20XGBoost%20%7C%20Pandas-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-green?style=for-the-badge)

> **Project Objective:** To build a robust classification-based machine learning model that predicts customer churn in subscription-based digital services. This project utilizes demographic, usage, and billing data to drive actionable retention strategies and reduce revenue loss.

---

## 📑 Table of Contents
1. [Project Overview](#-project-overview)
2. [Project Workflow (Flow Diagram)](#-project-workflow-flow-diagram)
3. [Dataset Description](#-dataset-description)
4. [Data Pre-processing & Visualization](#-data-pre-processing--visualization)
5. [Model Development](#-model-development--implementation)
6. [Performance Evaluation](#-performance-evaluation)
7. [Innovation & Creativity](#-innovation--creativity-approaches)
8. [Conclusion & Retention Strategies](#-conclusion--retention-strategies)
9. [How to Run](#-how-to-run)

---

## 🎯 Project Overview
**(Rubric: Problem Understanding & Objective Clarity - 5 Marks)**

Customer churn is a critical metric for subscription-based businesses. This project simulates an **IBM Watson Analytics** scenario where the goal is not just to predict *who* will leave, but *why*.

**Key Goals:**
* Analyze customer demographic and behavioral data.
* Identify key indicators of churn (e.g., contract type, payment method).
* Build a predictive model to classify users as "Likely to Churn" or "Retained".
* Provide business recommendations to improve customer retention.

---

## 🔄 Project Workflow (Flow Diagram)
**(Instruction: Flow diagram with Pre-processing and visualization steps clearly documented)**

The following pipeline illustrates our end-to-end methodology, from raw data to actionable insights:

```mermaid
graph TD
    A[<b>Data Collection</b><br/>Source: IBM Telco Dataset] --> B[<b>Data Pre-processing</b><br/>- Handle Null Values<br/>- Type Conversion<br/>- Outlier Detection]
    B --> C[<b>Feature Engineering</b><br/>- Binning Tenure<br/>- One-Hot Encoding<br/>- Scaling (MinMax)]
    C --> D[<b>Exploratory Data Analysis</b><br/>- Correlation Matrix<br/>- Churn Distribution<br/>- Bivariate Analysis]
    D --> E{<b>Class Imbalance?</b>}
    E -- Yes --> F[<b>SMOTE Optimization</b><br/>Synthetic Minority Oversampling]
    E -- No --> G[<b>Train/Test Split</b>]
    F --> G
    G --> H[<b>Model Development</b><br/>- Logistic Regression<br/>- Random Forest<br/>- XGBoost (Champion)]
    H --> I[<b>Evaluation</b><br/>- Confusion Matrix<br/>- ROC-AUC Curve<br/>- F1-Score]
    I --> J[<b>Conclusion</b><br/>Retention Strategies]
