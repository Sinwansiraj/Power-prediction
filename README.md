### Power-prediction
📌 Author - Sinwan Siraj
- Data Science Enthusiast | Energy Forecasting Specialist

# 🔋 PowerPulse: Household Energy Usage Forecasting

PowerPulse is a data science project focused on **predicting household power consumption** using time-series data and various machine learning models. It includes detailed data preprocessing, exploratory analysis, feature engineering, model training, hyperparameter tuning, and model evaluation.

---

## 📌 Project Objective

To build accurate regression models that forecast household energy usage using historical power consumption data and engineered time-based features. This supports energy planning, cost forecasting, and anomaly detection.

---

## 🗃️ Dataset

- Source: UCI Household Power Consumption dataset
- Duration: December 2006 – November 2010
- Granularity: 1-minute interval
- Size: ~2 million records
- Target variable: `Global_active_power`




---

## 🧠 ML Approach: Time Series Regression

- **Target Variable:** `Global_active_power`
- **Features Used:** Time-based (hour, day, week), sub-meter readings, voltage, intensity, and engineered rolling averages.
- **Models Evaluated:**
  - Linear Regression
  - Random Forest
  - LightGBM
  - MLP Regressor (Neural Network)

---

## 🛠️ Workflow

### 1. Data Preprocessing

- Parsed and combined `Date` & `Time` to create `Datetime` index.
- Converted object data to numeric with coercion for invalid entries.
- Interpolated missing values after reindexing to create a continuous time series.


df = df.reindex(full_datetime_index)
df = df.interpolate(method='time').ffill().bfill()
✅ Final shape: 2,075,259 minute-level rows with 17 engineered features.

2. Exploratory Data Analysis
📉 Full Period Trend

📆 Daily & Weekly Trends


🔍 Seasonal and annual cycles clearly emerge.

🕐 Hourly Pattern

✅ Peak hours: 7–9 AM, 7–9 PM
⬇️ Lowest usage: 2–4 AM

🔍 Correlation Matrix

⚠️ Global_intensity and Global_active_power are highly correlated (≈ 0.99)
⚠️ Multicollinearity addressed during model selection.

3. Feature Engineering
Feature Name	Description
hour, day_of_week, is_weekend	Time-based seasonality
Sub_metering_total, Global_active_power_other	Appliance-specific vs. other consumption
Global_active_power_roll_24h_mean	24-hour rolling average power

🤖 Models & Evaluation
📊 Train/Test Split
Chronological split:

Train: Before Oct 2009

Test: Oct 2009–Nov 2010

Why? Avoids data leakage, preserves temporal order, captures seasonality.

## 🧾 Evaluation Metrics

| Model                        | RMSE   | MAE    | R²      |
|-----------------------------|--------|--------|---------|
| Linear Regression           | 0.0388 | 0.0250 | 0.9984  |
| Random Forest (Tuned)       | 0.0283 | 0.0170 | **0.9991** ✅ |
| LightGBM (Tuned)            | 0.0302 | 0.0198 | 0.9990  |
| Neural Network (MLP, Tuned) | 0.0322 | 0.0222 | 0.9989  |

> ✅ **Best Model:** Random Forest (Tuned)

🔧 Hyperparameter Tuning
Method: GridSearchCV with TimeSeriesSplit

Best Models:

Random Forest: n_estimators=80, max_features=0.7, min_samples_leaf=5

LightGBM: n_estimators=150, learning_rate=0.05, num_leaves=20

MLP: hidden_layer_sizes=(50,), alpha=0.001


🏅 Best Model: Random Forest (Tuned)

💾 Model Export
python
Copy
Edit
import joblib
joblib.dump(best_rf_model, "power_prediction_rf_model.joblib")
📚 Future Work
Model ensembling (Voting/Stacking)

Long short-term memory (LSTM) networks for deeper temporal modeling

Integration with real-time IoT power monitoring

📸 Visual Folder Structure (Expected)

└── images/
    ├── daily_avg.png![daily_avg](https://github.com/user-attachments/assets/15cc5030-2caa-4679-b9b8-c6f54a2208b6)
    ├── hourly_avg.png![hourly_avg](https://github.com/user-attachments/assets/59475856-8d50-463e-8899-6882f288c1cb)
    └── correlation_matrix.png![correlation_matrix](https://github.com/user-attachments/assets/1d690b2d-21a9-4557-afb8-20edd6202e74)


🏁 Conclusion
PowerPulse demonstrates how smart feature engineering and model tuning can lead to high-accuracy forecasting in time series problems, especially in energy and utilities domains. This pipeline can be adapted for broader smart-home and IoT energy management syst
