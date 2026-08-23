# 🛒 Retail Demand Forecasting

A machine learning and time-series forecasting project designed to predict daily retail demand across stores and product families using historical sales patterns, seasonality, promotions, holidays, and external factors.

The project compares **Seasonal Naive, SARIMAX, and XGBoost forecasting approaches**, performs time-series feature engineering and model validation, and translates forecasting results into practical retail business insights.

---

## 📌 Project Overview

Accurate demand forecasting is important in retail because it supports:

* Inventory planning
* Stock replenishment
* Purchasing decisions
* Promotion planning
* Workforce and operational planning
* Reducing overstock and stockout risk

This project develops an end-to-end retail demand forecasting workflow using historical sales data.

The forecasting problem is defined at the:

> **Store × Product Family × Day**

level, with **daily sales** as the target variable.

The project focuses primarily on **one-day-ahead forecasting**, using information available up to day *t* to predict demand for day *t+1*.

---

## 🎯 Objectives

The main objectives of this project were to:

1. Understand historical retail sales patterns.
2. Explore temporal and seasonal demand behaviour.
3. Analyze the relationship between sales, promotions, holidays, and external factors.
4. Engineer time-series features suitable for machine learning.
5. Establish a Seasonal Naive baseline.
6. Develop SARIMAX and XGBoost forecasting models.
7. Tune and validate the machine learning model.
8. Compare forecasting performance using MAE, RMSE, and MAPE.
9. Analyze forecast errors and over/under-forecasting behaviour.
10. Translate the forecasting results into business insights.

---

## 📊 Dataset

The project uses the **Kaggle Store Sales** dataset.

The dataset combines historical retail sales with store information, promotions, holidays, transactions, and oil-price information.

### Dataset Components

| Dataset      |      Rows | Purpose                                 |
| ------------ | --------: | --------------------------------------- |
| Train        | 3,000,888 | Historical sales used for modelling     |
| Test         |    28,512 | Future observations used for evaluation |
| Stores       |        54 | Store-level information                 |
| Oil          |     1,218 | External oil-price information          |
| Holidays     |       350 | Holiday and event information           |
| Transactions |    83,488 | Store transaction information           |

### Main Variables

The primary sales dataset contains variables such as:

* `date`
* `store_nbr`
* `family`
* `sales`
* `onpromotion`

The processed forecasting dataset contains **3,000,888 observations across 13 variables** before the time-series feature engineering stage.

The historical period covers:

**2013-01-01 → 2017-08-15**

with:

* **54 stores**
* **33 product families**
* **1,684 unique dates**

---

## 🔄 Project Workflow

The project was developed as a structured 10-stage forecasting pipeline:

```text
Raw Retail Data
      ↓
01. Data Understanding
      ↓
02. Data Preprocessing
      ↓
03. Exploratory Data Analysis
      ↓
04. Time-Series Feature Engineering
      ↓
05. Baseline Forecasting
      ↓
06. SARIMAX Forecasting
      ↓
07. XGBoost Forecasting
      ↓
08. Model Tuning & Validation
      ↓
09. Final Forecasting
      ↓
10. Business Insights
```

---

## 🔍 1. Data Understanding

The first stage focused on understanding the structure and characteristics of the available datasets.

The analysis included:

* Dataset dimensions
* Data types
* Missing-value investigation
* Duplicate checks
* Date coverage
* Store coverage
* Product-family coverage
* Target-variable analysis
* Relationships between datasets

The target variable for forecasting is:

```text
sales
```

---

## 🧹 2. Data Preprocessing

The preprocessing stage prepared the raw retail data for forecasting.

Key activities included:

* Date conversion
* Sorting observations chronologically
* Combining relevant datasets
* Handling missing values
* Validating sales observations
* Preparing store and product-family information
* Preparing external variables
* Creating a modelling-ready dataset

The processed data was then used for the time-series feature engineering stage.

---

## 📈 3. Exploratory Data Analysis

EDA was performed to understand the underlying demand behaviour before modelling.

The analysis examined:

* Overall sales trends
* Daily demand behaviour
* Store-level sales
* Product-family sales
* Monthly patterns
* Weekly patterns
* Promotion effects
* Holiday effects
* Demand variability
* Correlations between variables

The business analysis showed that the final evaluation period had an average daily sales level of approximately **10,016 units**, with a standard deviation of approximately **1,366 units**.

---

## ⚙️ 4. Time-Series Feature Engineering

Time-series features were created to allow machine learning models to learn historical demand behaviour.

### Calendar Features

The project created:

* Year
* Month
* Quarter
* Week of year
* Day of month
* Day of week
* Weekend indicator

### Cyclical Features

To represent recurring seasonal patterns, cyclical transformations were created for:

* Month
* Day of week
* Week of year

using sine and cosine transformations.

### Lag Features

Historical demand was represented using lagged sales features:

```text
sales_lag_1
sales_lag_2
sales_lag_3
sales_lag_7
sales_lag_14
sales_lag_28
```

These features capture recent, weekly, and longer-term demand behaviour.

### Leakage Prevention

A key part of the feature engineering process was ensuring that historical demand features only used information available **before the prediction date**.

This prevents the model from accidentally using future sales information.

---

## 🤖 5. Forecasting Models

Three major forecasting approaches were evaluated.

### 1. Seasonal Naive

A seasonal naive approach was used as the baseline.

This provides a simple benchmark against which more complex forecasting approaches can be compared.

### 2. SARIMAX

SARIMAX was evaluated as a statistical time-series forecasting approach.

It was used to model temporal patterns and compare traditional statistical forecasting against machine-learning approaches.

### 3. XGBoost

XGBoost was used as the main machine-learning forecasting model.

The model used engineered temporal and historical-demand features to learn relationships between previous demand behaviour and future sales.

The project also included a separate tuning and validation stage for XGBoost.

---

## 📊 6. Model Evaluation

The models were evaluated using:

* **MAE — Mean Absolute Error**
* **RMSE — Root Mean Squared Error**
* **MAPE — Mean Absolute Percentage Error**

Lower values indicate better forecasting performance.

### Final Model Comparison

| Model             |        MAE |       RMSE |  Rank |
| ----------------- | ---------: | ---------: | ----: |
| 🥇 Seasonal Naive | **139.25** | **550.04** | **1** |
| XGBoost           |     641.55 |     780.44 |     2 |
| Tuned XGBoost     |     818.47 |   1,014.53 |     3 |
| SARIMAX           |   1,357.77 |   1,744.01 |     4 |

Based on the final MAE and RMSE comparison, the **Seasonal Naive model was selected as the final forecasting model**.

An important finding from the comparison is that a more complex model does not automatically provide better forecasting performance. In this dataset, the seasonal baseline captured the demand pattern particularly well.

---

## 🏆 Final Forecasting Model

### Selected Model: Seasonal Naive

Final evaluation results:

| Metric |     Result |
| ------ | ---------: |
| MAE    | **139.25** |
| RMSE   | **550.04** |
| MAPE   |  **8.42%** |

The selected model achieved an average absolute forecasting error of approximately **139 sales units** and a MAPE of approximately **8.42%**.

---

## 📉 Forecast Error Analysis

The project also examined the direction and magnitude of forecasting errors.

During the final evaluation:

* **18 days** were over-forecasted
* **12 days** were under-forecasted
* Average over-forecast error: **967.10**
* Average under-forecast error: **595.53**

This analysis is important from a business perspective because over-forecasting and under-forecasting can have different operational consequences.

### Business Interpretation

**Over-forecasting** can result in:

* Excess inventory
* Higher storage costs
* Increased risk of unsold stock

**Under-forecasting** can result in:

* Stockouts
* Lost sales
* Poor customer experience
* Missed revenue opportunities

Therefore, evaluating the *direction* of forecast errors is useful in addition to looking only at overall accuracy.

---

## 📦 Business Insights

The final business analysis focuses on converting forecasting results into practical retail decisions.

Potential applications include:

### Inventory Planning

Forecasted demand can help retailers determine how much inventory should be available for upcoming periods.

### Replenishment

Historical demand patterns and forecasts can support more timely stock replenishment.

### Promotion Planning

Understanding demand behaviour around promotional periods can help retailers evaluate the expected impact of promotions.

### Store-Level Planning

Demand differs across stores and product families, so forecasting at the store-product level allows more targeted planning.

### Risk Management

Forecast error analysis can help identify periods where the business is more likely to experience overstock or stockout risk.

---

## 📁 Project Structure

```text
Retail-Demand-Forecasting/
│
├── data/
│   ├── test.csv
│   ├── stores.csv
│   ├── oil.csv
│   ├── holidays_events.csv
│   └── transactions.csv
│
├── outputs/
│   └── tables/
│       ├── annual_sales_summary.csv
│       ├── baseline_family_results.csv
│       ├── baseline_results.csv
│       ├── business_demand_accuracy.csv
│       ├── business_metrics.csv
│       ├── business_monthly_sales.csv
│       ├── business_promotion_summary.csv
│       ├── business_weekday_sales.csv
│       ├── correlation_matrix.csv
│       ├── family_sales_summary.csv
│       ├── family_volatility.csv
│       ├── final_error_summary.csv
│       ├── final_forecast_summary.csv
│       ├── final_forecast_with_errors.csv
│       ├── final_model_comparison.csv
│       ├── final_xgboost_forecast.csv
│       ├── holiday_summary.csv
│       ├── largest_forecast_errors.csv
│       ├── promotion_summary.csv
│       ├── sarimax_forecast.csv
│       ├── sarimax_results.csv
│       ├── store_sales_summary.csv
│       ├── xgboost_feature_importance.csv
│       ├── xgboost_forecast.csv
│       ├── xgboost_results.csv
│       ├── xgboost_tuning_comparison.csv
│       └── xgboost_tuning_results.csv
│
├── 01_Data_Understanding.ipynb
├── 02_Data_Preprocessing.ipynb
├── 03_Exploratory_Data_Analysis.ipynb
├── 04_Time_Series_Feature_Engineering.ipynb
├── 05_Baseline_Forecasting.ipynb
├── 06_SARIMAX_Forecasting.ipynb
├── 07_XGBoost_Forecasting.ipynb
├── 08_Model_Tuning_Validation.ipynb
├── 09_Final_Forecasting.ipynb
├── 10_Business_Insights.ipynb
│
├── .gitignore
└── README.md
```

> Note: Large raw/processed datasets are excluded from the GitHub repository using `.gitignore` because of GitHub's file-size limitations.

---

## 🧰 Technologies Used

### Programming

* Python

### Data Analysis

* Pandas
* NumPy

### Visualization

* Matplotlib
* Seaborn

### Machine Learning

* XGBoost
* Scikit-learn

### Time-Series Forecasting

* SARIMAX
* Statistical forecasting techniques

### Development Environment

* Jupyter Notebook
* Visual Studio Code
* Git
* GitHub

---

## 📚 Notebook Guide

| Notebook                                   | Purpose                                              |
| ------------------------------------------ | ---------------------------------------------------- |
| `01_Data_Understanding.ipynb`              | Understand datasets and target variable              |
| `02_Data_Preprocessing.ipynb`              | Clean and prepare the data                           |
| `03_Exploratory_Data_Analysis.ipynb`       | Explore sales patterns and relationships             |
| `04_Time_Series_Feature_Engineering.ipynb` | Create temporal and historical demand features       |
| `05_Baseline_Forecasting.ipynb`            | Establish baseline forecasting performance           |
| `06_SARIMAX_Forecasting.ipynb`             | Develop and evaluate SARIMAX                         |
| `07_XGBoost_Forecasting.ipynb`             | Develop XGBoost forecasting model                    |
| `08_Model_Tuning_Validation.ipynb`         | Tune and validate forecasting models                 |
| `09_Final_Forecasting.ipynb`               | Generate and evaluate final forecasts                |
| `10_Business_Insights.ipynb`               | Translate forecasting results into business insights |

---

## ▶️ How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/SamadhiGamagedara-sg/Retail-Demand-Forecasting.git
cd Retail-Demand-Forecasting
```

### 2. Install required libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost statsmodels jupyter
```

### 3. Add the required datasets

The large dataset files are intentionally excluded from the repository.

Place the required files inside:

```text
data/
```

including the required training and supporting datasets.

### 4. Run the notebooks in order

Start with:

```text
01_Data_Understanding.ipynb
```

and continue through:

```text
10_Business_Insights.ipynb
```

Running the notebooks sequentially reproduces the complete forecasting workflow.

---

## 📌 Key Takeaways

### 1. Seasonal patterns matter

Retail demand contains recurring temporal patterns that can be highly informative for forecasting.

### 2. More complex models are not always better

The Seasonal Naive baseline outperformed both XGBoost variants and SARIMAX on the final MAE/RMSE comparison.

### 3. Feature engineering is important

Calendar, cyclical, lagged, promotional, holiday, and external features were incorporated to represent different sources of demand variation.

### 4. Forecast accuracy should be interpreted in a business context

MAE, RMSE, and MAPE provide quantitative measures of accuracy, but over-forecasting and under-forecasting have different operational consequences.

### 5. Forecasting can support better decisions

A forecasting system can provide useful information for inventory, replenishment, promotion, and operational planning.

---

## 🚀 Future Improvements

Potential improvements to the project include:

* Testing additional machine-learning algorithms
* Developing hierarchical forecasting across stores and product families
* Using ensemble forecasting
* Performing rolling-origin cross-validation
* Adding more advanced hyperparameter optimization
* Building prediction intervals for uncertainty estimation
* Developing an interactive forecasting dashboard
* Deploying the final forecasting pipeline as a web application or API
* Monitoring model performance over time

---

## 📈 Project Outcome

This project demonstrates an end-to-end approach to retail demand forecasting, from raw data understanding and preprocessing through time-series feature engineering, baseline modelling, statistical forecasting, machine learning, model validation, final forecasting, and business interpretation.

Rather than assuming that the most complex model is automatically the best, the project uses model comparison and validation to identify the approach that performed best on the selected evaluation criteria.

**Final selected approach: Seasonal Naive forecasting**

**Final MAPE: 8.42%**

**Final MAE: 139.25**

**Final RMSE: 550.04**

---

## 👤 Author

**Samadhi Gamagedara**

Data Science / Machine Learning Portfolio Project

GitHub: [SamadhiGamagedara-sg](https://github.com/SamadhiGamagedara-sg)

---

## ⭐ If you found this project useful

Feel free to explore the notebooks and forecasting outputs to understand the complete modelling workflow.
