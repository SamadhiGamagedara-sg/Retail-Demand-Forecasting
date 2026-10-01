# Retail Demand Forecasting

End-to-end time-series forecasting on the Kaggle [Store Sales](https://www.kaggle.com/competitions/store-sales-time-series-forecasting) dataset. The project covers data preparation, EDA, feature engineering, statistical and machine-learning models, tuning, error analysis, and business interpretation.

Models are compared on the highest-volume series (**Store 45, GROCERY I**), forecasting daily sales one day ahead.

## Key Results

- **XGBoost was the best model**, with a **48.3% lower MAE** than the Seasonal Naive benchmark.
- **Hyperparameter tuning made XGBoost worse**, so the tuned configuration did not generalize beyond the validation split.
- **SARIMAX was the weakest model**, performing worse than the Seasonal Naive benchmark.

| Rank | Model | MAE | RMSE |
|---|---|---|---|
| 1 | XGBoost | 677.56 | 795.69 |
| 2 | Tuned XGBoost | 834.37 | 1,047.69 |
| 3 | Seasonal Naive | 1,309.86 | 1,656.28 |
| 4 | SARIMAX | 2,070.18 | 2,626.44 |

*All models scored on the same 22 test days (2017-07-17 to 2017-08-15). Lower is better.*

## Dataset

- 3,000,888 training rows, 2013-01-01 to 2017-08-15, 54 stores, 33 product families
- Supporting data: stores, oil prices, holidays, transactions
- The Kaggle test file has no sales values, so all evaluation uses chronological holdouts from the training data

## Approach

**Features (36):** calendar and cyclical encodings, sales lags (1, 2, 3, 7, 14, 28 days), rolling means and standard deviations, promotion, holiday, transaction and oil-price features. All history-based features use lagged values to prevent leakage.

**Models**
- **Seasonal Naive:** sales from the same weekday one week earlier
- **SARIMAX:** `(1,1,1)(1,1,1,7)` with promotion and oil price as exogenous variables
- **XGBoost:** lag, rolling and calendar features; 500 trees, depth 8, learning rate 0.05
- **Tuned XGBoost:** five configurations compared on a validation split

**Baseline across all series (validation Jan to Jun 2017):** Naive MAE 139.25, Seasonal Naive MAE 102.19. These are averaged over all 1,782 series and are not comparable to the single-series table above.

**XGBoost error analysis (30-day holdout):** MAE 641.55, RMSE 780.44, MAPE 6.57%. The model over-forecast on 18 days (average error 685) and under-forecast on 12 days (average error 576).

## Business Insights

- **Weekly pattern:** Sunday (13,961) and Saturday (12,153) are the busiest days. Thursday (7,112) is the quietest.
- **Promotions:** average sales on promotion days were 31.1% higher than on other days (an association, not adjusted for weekday effects).
- **Error direction:** over-forecasting raises holding costs and under-forecasting risks stockouts, so safety stock should account for the direction of error, not just its size.

## Limitations

- Comparison covers one series and a short holdout, so results may not generalize to other stores or families.
- The XGBoost holdout contains weekdays only, so the common-window comparison covers weekdays only.
- XGBoost and Seasonal Naive use observed lags (one-step-ahead), while SARIMAX produces a 30-step forecast, so its result is not strictly like-for-like.
- Tuning used five configurations and a single validation split, with no rolling-origin cross-validation.


## Author

**Samadhi Gamagedara** · [GitHub](https://github.com/SamadhiGamagedara-sg)
