# Datheon Data Analytics & Power BI Internship

Submission by **Sneha Yadav**. Four tasks, built on three public datasets (Superstore Sales, IBM Telco Customer Churn, Microsoft Financial Sample).

| Task | Topic | Tools | Deliverable | Folder |
|---|---|---|---|---|
| 1 | Exploratory Data Analysis on Superstore Retail Sales | Python, Pandas, Seaborn | Jupyter notebook with cleaning code and 7 annotated business findings | `Task_1_Superstore_EDA/` |
| 2 | Interactive Power BI Executive Sales & Profit Dashboard | Power BI, DAX | .pbix file and PDF report | `Task_2_PowerBI_Superstore_Dashboard/` |
| 3 | Customer Cohort & Churn Analytics | Python, Pandas, SciPy | Cohort retention matrix, churn report, retention recommendations | `Task_3_Churn_Cohort_Analysis/` |
| 4 | Corporate Financial & Margin Performance Dashboard | Power BI, DAX | .pbix file with budget variance and scenario sliders | `Task_4_Financial_Margin_Dashboard/` |

## Headline results

**Superstore (Tasks 1 and 2):** $2.30M sales, $286K profit, 12.5% margin. Sales grew 51% from 2015 to 2018. Discounts above 20% are 14% of order lines but lost $135K, nearly half of total profit. Furniture is 32% of sales but only 6.4% of profit.

**Telco churn (Task 3):** 26.5% of customers churned. 55% of all churn happens in the first 12 months. Month-to-month contracts churn at 42.7% against 2.8% for two-year contracts. Fiber optic customers without tech support churn at 49.4%.

**Financial Sample (Task 4):** $118.7M net sales and $16.9M profit (14.2% margin). The Enterprise segment sells below cost (-3.1% margin). Actual discounts of 7.2% are above a 5% plan.

## Repository structure
```
data/                                  Source datasets
Task_1_Superstore_EDA/                 Notebook
Task_2_PowerBI_Superstore_Dashboard/   Data model, DAX, build guide, expected values, .pbix, PDF
Task_3_Churn_Cohort_Analysis/          Notebook, report, charts
Task_4_Financial_Margin_Dashboard/     Data model, DAX, build guide, expected values, .pbix
```

## How to run the notebooks
```
pip install pandas numpy matplotlib seaborn scipy jupyter
jupyter notebook
```
Open the notebook inside its task folder. The notebooks read data from `../data/`.

## Notes
- Task 1 found that the Superstore CSV has two small tables (People and Returns) pasted below the orders. They were separated, not treated as missing data.
- Task 4 has no budget column in the source data, so the budget is an adjustable assumption (5% discount rate, 85% COGS ratio).
