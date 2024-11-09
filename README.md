# Churn Analysis Portfolio Project Documentation

## Introduction
This document outlines a portfolio project focused on end-to-end churn analysis, a crucial process for businesses across industries to understand and mitigate customer attrition. The project uses a telecom company dataset to demonstrate the application of data analysis, visualization, and machine learning techniques to gain insights into customer churn and predict future churners. The insights derived are applicable across various industries that prioritize customer retention.

## Project Objectives
This project aims to achieve the following:
- **Data Analysis**: Visualize and analyze customer data at different levels, including demographic, geographic, payment and account information, and services used.
- **Churner Profile**: Develop a profile of customers who have churned to understand the key drivers and characteristics of churn.
- **Marketing Strategies**: Identify specific areas where targeted marketing campaigns can be implemented to improve customer retention.
- **Churn Prediction**: Develop a method to predict which customers are likely to churn in the future, allowing for proactive intervention.

## Metrics
The project focuses on the following key metrics:
- **Total Customers**: The total number of customers in the dataset.
- **Total Churn**: The total number of customers who have churned.
- **Churn Rate**: The percentage of customers who have churned, calculated as Total Churn divided by Total Customers.
- **New Joiners**: The total number of new customers who have joined.

## Project Methodology
The project follows a six-step approach:

### 1. ETL Process in SQL Server
- This step involves extracting, transforming, and loading the customer data into a SQL Server database, chosen for its robust capabilities in handling data and maintaining integrity.
- **Steps**:
  - Install SQL Server Management Studio (SSMS).
  - Create a database named `db_Churn`.
  - Import the CSV data file into a staging table (`stg_Churn`) using the Import Wizard. Set `customerId` as the primary key and adjust the data type of "Bit" columns to `Varchar(50)` to prevent errors.
  - Perform data exploration to analyze column values (e.g., gender, contract, customer status, state) and identify null values.
  - Remove null values from `stg_Churn` and insert the cleaned data into a production table (`prod_Churn`).
  - Create views for Power BI – `vw_ChurnData` (for churned and stayed customers) and `vw_JoinData` (for new customers).

### 2. Power BI Data Transformation
- The data from the SQL Server database is further transformed within Power BI for visualization and analysis.
- **Steps**:
  - Add new columns to `prod_Churn`:
    - **Churn Status**: A binary column (1 for churned, 0 for stayed) based on `Customer_Status`.
    - **Monthly Charge Range**: Categorizes monthly charges (e.g., "< 20", "20-50", "50-100", "> 100").
  - Create new tables:
    - **mapping_AgeGrp**: Groups ages into categories (e.g., "< 20", "20 – 35", "36 – 50", "> 50") with a sorting column (`AgeGrpSorting`).
    - **mapping_TenureGrp**: Groups tenure into categories (e.g., "< 6 Months", "6-12 Months") with a sorting column (`TenureGrpSorting`).
    - **prod_Services**: Unpivots service columns (e.g., "Phone_Service", "Internet_Service") in `prod_Churn` to list each service for customers.

### 3. Power BI Measure Creation
- Define measures in Power BI to calculate key metrics for visualization.
- **Measures**:
  - `Total Customers`: `COUNT(prod_Churn[Customer_ID])`
  - `New Joiners`: `CALCULATE(COUNT(prod_Churn[Customer_ID]), prod_Churn[Customer_Status] = "Joined")`
  - `Total Churn`: `SUM(prod_Churn[Churn Status])`
  - `Churn Rate`: `[Total Churn] / [Total Customers]`

### 4. Power BI Historical Data Visualization
- Create a Summary Page dashboard in Power BI to visualize historical data and provide an overview of customer churn.
- **Visualizations**:
  - **Cards**: Display metrics like Total Customers, New Joiners, Total Churn, and Churn Rate.
  - **Donut Chart**: Visualize churn rate by gender.
  - **Line and Stacked Column Chart**: Compare total customers and churn rate across age groups.
  - **Clustered Bar Charts**: Analyze churn rate by payment method, contract type, and internet type.
  - **Matrix**: Visualize churn status (percentage) for services (e.g., phone service).
  - **Dropdowns (Slicers)**: Filter data by monthly charge range and marital status.

### 5. Customer Churn Prediction with Random Forest
- Build a machine learning model using the Random Forest algorithm to predict customer churn.
- **Steps**:
  - Data Preparation: Import `vw_ChurnData` and `vw_JoinData` into an Excel file named `Prediction_Data`.
  - Environment Setup: Install Anaconda and necessary Python libraries using pip.
  - Model Building (Jupyter Notebook):
    - Import libraries (`pandas`, `numpy`, `matplotlib`, `scikit-learn`).
    - Load data into a pandas DataFrame, preprocess features, split data, and train the model.
  - Prediction on New Data: Preprocess and predict churn on `vw_JoinData` and save the results to `Predictions.csv`.

### 6. Power BI Churn Prediction Visualization
- Import `Predictions.csv` containing the churn predictions into Power BI.
- **Steps**:
  - Create a new "Churn Prediction" page in Power BI.
  - **Measures**:
    - `Count Predicted Churner`: `COUNT(Predictions[Customer_ID])`
  - **Visualizations**:
    - **Cards**: Display total number of predicted churners.
    - **Clustered Column Chart**: Show count of predicted churners across age groups.
    - **Table**: Present detailed information for predicted churners.

## Project Outcome
The project results in a comprehensive Power BI dashboard with two primary pages:
- **Summary Page**: Provides an executive summary of historical churn data, enabling analysis across customer dimensions.
- **Churn Prediction Page**: Displays churn prediction model results, presenting a profile of predicted churners.

This dashboard provides stakeholders with actionable insights to understand churn drivers, identify at-risk customers, and strategize targeted interventions to improve customer retention.
