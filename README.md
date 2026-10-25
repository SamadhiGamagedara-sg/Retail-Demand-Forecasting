# Retail Demand Forecasting Using Time Series Analysis & Machine Learning

An end-to-end retail demand forecasting project using the [Kaggle Store Sales – Time Series Forecasting dataset](https://www.kaggle.com/competitions/store-sales-time-series-forecasting). This project explores historical sales patterns, develops predictive features, compares statistical and machine learning models, and evaluates forecasting performance to support data-driven retail inventory planning.

The primary model comparison focuses on **Store 45 – GROCERY I**, forecasting daily sales one day ahead.

## Project Highlights

- Developed an end-to-end time-series forecasting workflow, from data preparation to model evaluation.
- Engineered 36 features, including lagged sales, rolling statistics, calendar variables, promotions, holidays, transactions, and oil prices.
- Compared Seasonal Naive, SARIMAX, XGBoost, and tuned XGBoost models.
- Evaluated prediction errors using Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE).
- Analyzed forecast errors and retail business patterns to identify potential inventory-planning implications.

## Model Performance

XGBoost achieved the lowest MAE among the four models, outperforming the Seasonal Naive benchmark by approximately **48.3% in MAE**.

| Rank | Model | MAE | RMSE |
|---:|---|---:|---:|
| 1 | XGBoost | 677.56 | 795.69 |
| 2 | Tuned XGBoost | 834.37 | 1,047.69 |
| 3 | Seasonal Naive | 1,309.86 | 1,656.28 |
| 4 | SARIMAX | 2,070.18 | 2,626.44 |

*Evaluation note: Results use the same 22 test observations from July 17 to August 15, 2017. Lower MAE and RMSE indicate better predictive performance. The evaluation window contains weekdays only.*

### Key Findings

- **XGBoost performed best:** It substantially reduced forecasting errors compared with the seasonal benchmark.
- **Tuning did not improve performance:** The tuned configuration performed worse than the original XGBoost model, highlighting the importance of validation and generalization.
- **SARIMAX underperformed:** It produced higher errors than the Seasonal Naive benchmark in this experiment.

## Dataset Overview

The project uses the [Kaggle Store Sales dataset](https://www.kaggle.com/competitions/store-sales-time-series-forecasting), which contains retail sales information from Ecuador.

- **Training records:** 3,000,888 rows
- **Time period:** January 1, 2013 – August 15, 2017
- **Stores:** 54
- **Product families:** 33
- **Supporting data:** Store metadata, oil prices, holidays and events, and transactions

The competition's test dataset does not contain observed sales targets. Therefore, model evaluation was conducted using chronological holdouts from the training data.

## Methodology

### 1. Data Preparation and Exploratory Data Analysis

- Examined sales distributions, trends, seasonality, and store-level demand patterns.
- Investigated the relationships between sales, promotions, weekdays, and other supporting variables.
- Prepared chronological training, validation, and testing periods.

### 2. Feature Engineering

Created 36 predictive features covering:

- **Time features:** Day, month, weekday, and cyclical calendar encodings.
- **Lag features:** Previous sales values at 1, 2, 3, 7, 14, and 28 days.
- **Rolling statistics:** Historical rolling means and standard deviations.
- **External variables:** Promotions, holidays, transactions, and oil prices.

History-based features use past observations to reduce the risk of data leakage.

### 3. Forecasting Models

| Model | Description |
|---|---|
| Seasonal Naive | Uses sales from the same weekday in the previous week. |
| SARIMAX | Statistical time-series model with promotion and oil price as external variables. |
| XGBoost | Gradient-boosted trees trained on engineered time-series features. |
| Tuned XGBoost | Evaluates five hyperparameter configurations using a validation split. |

The XGBoost configuration used 500 trees, a maximum depth of 8, and a learning rate of 0.05. SARIMAX used the configuration `(1,1,1)(1,1,1,7)`.

### 4. Model Evaluation and Error Analysis

Models were evaluated using MAE and RMSE. Additional analysis examined forecast bias and the potential implications of over-forecasting and under-forecasting.

**Additional XGBoost error analysis:**

- MAE: 641.55
- RMSE: 780.44
- MAPE: 6.57%
- Over-forecasting: 18 days
- Under-forecasting: 12 days

These metrics come from a separate 30-day holdout and should not be directly compared with the 22-observation model comparison table.

## Business Insights

- **Weekly demand patterns:** Sunday recorded the highest average sales (13,961), followed by Saturday (12,153). Thursday had the lowest average sales (7,112).
- **Promotion impact:** Average sales on promotion days were 31.1% higher than on non-promotion days. This is an observed association and does not establish a causal effect.
- **Inventory planning:** Over-forecasting may increase holding costs, while under-forecasting may contribute to stockouts. Understanding forecast error direction can help inform inventory and replenishment decisions.

## Limitations

- The main model comparison focuses on one store and product family, limiting generalizability.
- The common evaluation window contains weekdays only.
- XGBoost and Seasonal Naive use observed lagged sales for one-step-ahead predictions, whereas SARIMAX generates a multi-step forecast. This difference limits strict comparability.
- Hyperparameter tuning used five configurations and a single validation split rather than rolling-origin cross-validation.
- Performance on other stores, product families, and future periods requires further evaluation.

## Technologies Used

**Python** · **Pandas** · **NumPy** · **Scikit-learn** · **XGBoost** · **Statsmodels** · **Matplotlib** · **Seaborn** · **Time Series Analysis** · **Feature Engineering** · **Model Evaluation**

## Author

**Samadhi Gamagedara**

[GitHub Profile](https://github.com/SamadhiGamagedara-sg)
