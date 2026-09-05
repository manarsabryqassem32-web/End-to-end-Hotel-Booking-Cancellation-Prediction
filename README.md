# Predicting Hotel Booking Cancellations 🏨

A machine learning project (Samsung Innovation Campus – AI803) that predicts whether a hotel booking will be cancelled, using only information available at the time of booking — helping hotels move from guesswork to data-driven demand forecasting.

## 📌 Problem Statement

Hotel cancellations lead to unsold rooms, wasted staffing, and inaccurate demand forecasts. Without knowing which bookings are likely to cancel, hotels are left overbooking based on guesswork rather than data.

This project builds a **binary classification model** that predicts whether a booking will be canceled, answering the question:

> *Which booking and customer characteristics are associated with booking cancellation?*

## 🎯 Who This Is For

- **Hotel revenue & operations managers** — anticipate cancellations to adjust pricing and staffing
- **Reservations & front-desk teams** — decide which bookings need a confirmation call or reminder
- **Overbooking strategy owners** — replace guesswork with a data-backed risk score

## 📊 Dataset

- **Source:** Hotel Booking Demand dataset (`hotel_bookings.csv`)
- **Size:** 119,390 bookings × 32 original features
- **Key variables:** hotel type, lead time, arrival date, stay length, guests, deposit type, market segment, customer type, ADR, special requests
- **Target variable:** `is_canceled` (1 = canceled, 0 = not canceled)

**Data quality issues found during inspection:**
- 31,994 exact duplicate rows
- Missing values in `company` (82,137), `agent` (12,193), `country` (452), `children` (4)
- Invalid records: 166 bookings with zero guests, 651 with zero nights, 1 with negative ADR

## 🛠️ Approach

**1. Data Preprocessing**
- Removed duplicates and invalid records (zero guests/nights, negative ADR)
- Handled missing values and addressed outliers
- Leakage prevention: removed `reservation_status` and `reservation_status_date` to avoid predicting the target from itself

**2. Feature Engineering**
- Created `total_guests`, `total_nights`, `total_stay_cost`, `is_family`, `is_weekend_stay`, `season`

**3. Data Transformation**
- One-hot encoding for categorical features; frequency encoding for the high-cardinality `country` column
- Tested `log1p` transformation on skewed continuous features (lead_time, adr, days_in_waiting_list, total_nights, total_stay_cost) — kept only where it improved F1-score / ROC-AUC

**4. Exploratory Data Analysis**
- Univariate & bivariate analysis of cancellation rate vs. hotel type, lead time, deposit type, market segment, customer type, repeat guests, special requests, and ADR
- Correlation heatmap to visualize relationships between key features

## 🤖 Models

Three supervised classification models were trained and compared:

| Model | Description |
|---|---|
| Logistic Regression | Linear baseline — fast, interpretable, scaled features |
| Random Forest | Ensemble of decision trees — captures non-linear feature interactions |
| XGBoost | Gradient boosting — strong performance on imbalanced tabular data |

## 📈 Results

| Metric | Best Model | Score |
|---|---|---|
| Accuracy | Random Forest | **0.834** |
| F1-Score | XGBoost | **0.705** |
| ROC-AUC | XGBoost | **0.899** |

Log transformation had minimal impact on test-set performance across all three models, so the without-log version was selected for simplicity.

## 🔑 Key Findings

1. **Cancellation Rate:** 27.7% of bookings were cancelled.
2. **Lead Time:** Longer lead times were associated with higher cancellation rates.
3. **Booking Patterns:** City Hotel, Online TA, and Transient bookings showed relatively higher cancellation rates.
4. **Guest Behavior:** Repeat guests and guests with more special requests tended to cancel less.
5. **Model Performance:** XGBoost achieved the best F1-Score and ROC-AUC; Random Forest achieved the best Accuracy.
6. **Geographic Finding:** Angola recorded a notably high cancellation rate of 56.6% across 341 bookings.

## 🚀 Deployment

The final model was deployed as an interactive web application, allowing users to input booking details and get a real-time cancellation prediction.

## 🔮 Future Recommendations

1. **Try additional models** — gradient boosting variants (LightGBM, CatBoost) or a stacked ensemble
2. **Hyperparameter tuning** — grid search or Bayesian optimization beyond default settings
3. **Incorporate external data** — local events, holidays, seasonal pricing trends
4. **Deploy as a real-time risk score** — a live scoring system flagging high-risk bookings as they're made, rather than one-time batch prediction

## 👥 Team

- Nardy Abdalla
- Manar Sabry
- Hussein Muhammed
- Mohamed Samy

**Facilitator:** Abdulrahman Mohamed

---
*Samsung Innovation Campus — SIC AI803*
