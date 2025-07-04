#  Energy Consumption Forecasting with Temperature Data

This project forecasts thermal energy consumption based on outdoor temperature using multiple modeling approaches. It is based on web anonymous data.

##  Project Goals

- Predict short-term energy usage (hourly and daily).
- Evaluate how temperature affects consumption.
- Support operational planning min. 24h ahead.
- Compare classical, machine learning, and deep learning models.

##  Data Description

Two datasets were used:
- `Temperatura.csv` – hourly outdoor temperature [°C].
- `Energia_Cieplna.csv` – hourly thermal energy usage [GJ].

After merging and cleaning:
- Zero/abnormal values were removed.
- Outliers handled using IQR and z-score.
- Features engineered: rolling means, lags, calendar variables.

##  Models Implemented

### 1. **Classical Time Series Models**
- `ARIMA`, `ARIMAX`, `SARIMAX`
- Stationarity tested with ADF
- Exogenous variable: temperature
- Better performance when temperature included

### 2. **XGBoost Regression**
- Full feature engineering pipeline:
  - Lags (up to 30h or 72h for 24h-ahead forecasting)
  - Rolling means
  - Time-based features (hour, dayofweek, month, etc.)
- **Excellent accuracy on hourly data**
- Extended to **day-ahead forecasting** by restricting lag features

### 3. **LSTM Neural Network**
- Multistep model
- Sequence length: 4 hours
- Final model saved as `best_lstm.h5`
- Predicts 4h span of energy usage from previous values (from 72h)
- Needs to be improved for more accurate predictions or/and predictions over longer time span

##  Evaluation

| Model      | Granularity | RMSE     | Notes |
|------------|-------------|----------|-------|
| ARIMAX     | Hourly      | ~7.9     | Captures trend but lacks variance |
| XGBoost    | Hourly      | ~0.18    | Excellent short-term prediction |
| SARIMAX    | Daily       | ~96      | Better than hourly ARIMA for daily |
| XGBoost    | Daily       | ~107     | Acceptable, but less accurate |
| LSTM       | 4-Hourly    | ~4.0     | Needs tuning; overcomes lag restriction |

### Feature Importance (XGBoost):
- **Most important:** `energy_lag_1` (for hourly), `energy_MA_7` and `energy_lag_1d` (for daily)
- Temperature had **limited standalone importance** but helped explain variance.

##  Visual Results

Plots comparing:
- Actual vs predicted energy consumption (hourly & daily)
- Feature importance from XGBoost
- Correlation analysis (Pearson & Spearman)
- Rolling patterns and seasonality

##  How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/Waldemar2P/energy-forecasting.git
   cd energy-forecasting
