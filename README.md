# Chocolate Sales Analysis and Prediction Project

This repository contains a comprehensive analysis of the Chocolate Sales dataset, covering end-to-end machine learning workflow from data cleaning to model deployment insights. The project predicts sales amount using regression models, with a focus on temporal data and categorical encoding.

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Technologies](#technologies)
- [Project Structure](#project-structure)
- [Data Workflow](#data-workflow)
  - [1. Data Loading and Inspection](#1-data-loading-and-inspection)
  - [2. Data Cleaning](#2-data-cleaning)
  - [3. Exploratory Data Analysis (EDA)](#3-exploratory-data-analysis-eda)
  - [4. Feature Engineering](#4-feature-engineering)
  - [5. Modeling](#5-modeling)
  - [6. Evaluation](#6-evaluation)
  - [7. Feature Importance](#7-feature-importance)
  - [8. Stretch Goals](#8-stretch-goals)
- [Results](#results)
- [Key Insights](#key-insights)
- [Future Work](#future-work)
- [Contributing](#contributing)
- [License](#license)

## Overview
The goal of this project is to analyze transactional chocolate sales data and build a predictive model for the `amount` of sales. Through data cleaning, exploration, and machine learning, we identify patterns in sales across countries, products, and time. The final Random Forest model provides actionable insights for business decisions.

## Dataset
- **Source**: Chocolate_Sales.csv from RosEdu workshop.
- **Description**: Contains transactional sales data with features like date, country, product, salesperson, boxes_shipped, and amount.
- **Size**: Approximately 1000 rows after cleaning.
- **Key Columns**:
  - `date`: Sale date (converted to datetime).
  - `amount`: Target variable (sales value).
  - `boxes_shipped`: Number of boxes (numeric).
  - `country`, `product`, `sales_person`: Categorical features.

## Technologies
- **Python 3.8+**
- **Libraries**:
  - Data Handling: Pandas, NumPy
  - Visualization: Matplotlib
  - Machine Learning: Scikit-learn (models, preprocessing, evaluation)
  - Development: Jupyter Notebook
- **Tools**: Git for version control.

## Project Structure
```
DataWorkshopRosEdu-ChocoCo/
├── data/
│   ├── raw/
│   │   └── Chocolate_Sales.csv          # Raw dataset
│   └── processed/
│       ├── Chocolate_Sales_Curated.csv  # Cleaned data
│       └── X_encoded.csv                # Feature-encoded data
├── notebooks/
│   └── chocolate_sales_analysis.ipynb   # Main analysis notebook
├── reports/
│   ├── figures/                         # Generated plots (PNG)
│   └── model_report.md                  # Auto-generated summary report
├── src/                                 # (Optional) Custom scripts
├── requirements.txt                     # Python dependencies
├── README.md                            # This file
└── .gitignore                           # Ignore data files, etc.
```

## Data Workflow

### 1. Data Loading and Inspection
- Load data: 
  ```python
  df = pd.read_csv('data/raw/Chocolate_Sales.csv')
  ```
- Inspect: 
  ```python
  df.head()
  df.info()
  df.describe()
  ```
- Standardize columns: Convert to lowercase, replace spaces with underscores.

### 2. Data Cleaning
- Remove duplicates: 
  ```python
  df.drop_duplicates(inplace=True)
  ```
- Handle missing values: Drop rows with missing `amount` or invalid dates.
- Convert dates: 
  ```python
  df['date'] = pd.to_datetime(df['date'], errors='coerce')
  ```
- Filter NaT: 
  ```python
  df = df.dropna(subset=['date'])
  ```
- Check for outliers: Identify `boxes_shipped <= 0` as data quality issues.

### 3. Exploratory Data Analysis (EDA)
- Missing values check: 
  ```python
  df.isna().sum()
  ```
- Summary stats: 
  ```python
  df.describe()
  ```
- Groupings:
  - Revenue by country: 
    ```python
    df.groupby('country')['amount'].sum().sort_values(ascending=False)
    ```
  - Revenue by product: 
    ```python
    df.groupby('product')['amount'].sum().sort_values(ascending=False)
    ```
  - Revenue by salesperson: 
    ```python
    df.groupby('sales_person')['amount'].sum().sort_values(ascending=False)
    ```
- Time analysis: Monthly revenue aggregation using `df['date'].dt.to_period('M')`
- Visualizations:
  - Revenue over time (line plot)
  - Top 10 products by revenue (horizontal bar chart)
  - Boxes shipped vs. revenue (scatter plot)

### 4. Feature Engineering
- Date features: `month`, `day_of_week`, `is_weekend`
- Target: `amount`
- Inputs: Numeric (boxes_shipped, month, day_of_week), Categorical (country, product, sales_person)
- Encoding: One-hot encoding with `OneHotEncoder` or `pd.get_dummies`
- Output: `X_encoded` DataFrame with encoded features.

### 5. Modeling
- Train/Test Split: Time-based (80/20) to respect temporal order.
- Baselines: Mean and median predictions from training set.
- Models:
  - Ridge Regression (with StandardScaler for features)
  - Random Forest Regressor
  - Cross-validation: TimeSeriesSplit for temporal data.

### 6. Evaluation
- Metrics: MAE, RMSE, R²
- Baseline comparison: Measure improvement.
- Error analysis: MAE per country/product, visualize error distributions.

### 7. Feature Importance
- Built-in (RF): `rf.feature_importances_`
- Permutation: `permutation_importance` for robustness.

### 8. Stretch Goals
- Add `is_weekend` feature.
- Predict monthly totals: Aggregate and model as time series.
- Try Gradient Boosting.
- Generate Markdown report with key figures and insights.

## Results
- **Baseline MAE**: ~3800
- **Random Forest MAE**: ~3400 (improvement of ~400)
- **R²**: ~0.3 (explains 30% of variance)
- **Top Features**: boxes_shipped, month, country_USA
- **Error Analysis**: Higher errors in countries/products with sparse data.
- All plots and reports are saved in `reports/`.

## Key Insights
- Sales patterns vary by country and product, with temporal trends.
- Boxes shipped correlates weakly with amount, indicating other factors (e.g., pricing).
- Model limitations: Small dataset leads to moderate performance; more data/features needed.
- Business recommendations: Focus on top countries (UK, USA) and products ("Dark Truffles").

## Future Work
- Incorporate external data (e.g., holidays, economic indicators).
- Experiment with advanced models (e.g., LSTM for time series).
- Hyperparameter tuning with GridSearchCV.
- Deploy model as a web API using Flask.

## Contributing
Contributions are welcome! Please fork the repo, create a branch, and submit a pull request. Ensure code follows PEP8 and includes tests.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
