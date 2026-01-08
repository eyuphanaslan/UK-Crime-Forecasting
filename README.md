# UK-Crime-Forecasting
Data science project for Udacity: Analyzing and forecasting UK crime trends.

📌 Project Overview

This project analyses and forecasts crime trends using UK monthly crime data across multiple crime categories.
It applies time-series analysis, machine learning, and uncertainty estimation to produce interpretable, decision-ready forecasts.

The project is designed as a serious applied data science pipeline, not a toy ML example.

🎯 Objectives

Understand historical crime patterns (trend & seasonality)

Compare multiple forecasting models per crime type

Quantify uncertainty (min–max / confidence ranges)

Perform rolling (walk-forward) evaluation

Forecast future crime volumes responsibly

Enable interactive exploration of model behaviour

Prepare ground for spatial and policy-level analysis

🗂 Dataset Summary
Data Characteristics

Time span: 36 months

Frequency: Monthly

Crime types: 14 categories

Raw granularity: Incident-level data

Model input: Aggregated monthly counts per crime type

Core Columns Used
Column	Description
Month	Time index (YYYY-MM)
Crime type	Crime category
count	Monthly incident count

Spatial fields (latitude/longitude) exist but are handled in separate spatial analysis chapters.

🧠 Why This Is a Time-Series Problem

Crime data violates many assumptions of standard ML:

Strong annual seasonality

Non-stationarity (policy, economy, social changes)

Structural breaks (e.g. COVID, enforcement changes)

Highly uneven variance across crime types

Therefore:

Random train/test splits are invalid

Cross-validation must respect time

Forecasts must include uncertainty

🔁 Backtesting vs Forecasting (Critical Concept)
Backtesting (Evaluation)

Uses last 6 months

Purpose: measure accuracy against known reality

Metrics: MAE, RMSE, R²

Only possible where actual data exists

Forecasting (Projection)

Predicts next 12 months

No true accuracy metrics possible

Must include uncertainty bands

6 months is a deliberate, professional choice:

Recent enough to reflect current dynamics

Long enough for meaningful evaluation

Preserves sufficient training data

🔄 Rolling / Walk-Forward Learning

Instead of a single static model fit, the project uses:

Expanding Window Strategy

Train on months 1–24 → test on 25–30

Expand to months 1–30 → test on 31–36

Train on all data → forecast months 37–48

This simulates real-world model deployment, where models improve as new data arrives.

This approach is standard in:

Finance

Energy forecasting

Epidemiology

Crime analytics

🧩 Crime-by-Crime Analysis

The system enforces single-crime analysis per run:

Prevents incorrect cross-crime assumptions

Improves interpretability

Allows tailored model selection

Crime types behave very differently:

Some are stable & seasonal

Some are sparse & volatile

Some are policy-sensitive

A one-size-fits-all model is inappropriate.

🤖 Forecasting Models Used

Each model represents a different hypothesis about crime behaviour.

1️⃣ Seasonal Naive (Baseline)

Assumption:

“This month ≈ same month last year”

Extremely strong baseline

Used to detect over-engineering

If a model cannot beat this, it is rejected

2️⃣ SARIMA (Statistical Time-Series)

Strengths

Native confidence intervals

Interpretable components (trend/seasonality)

Weaknesses

Sensitive to non-stationarity

Can produce explosive forecasts

Exploding forecasts are signals, not bugs.

3️⃣ Linear Regression

Simple and interpretable

Captures trend + seasonality

Sanity check for complexity

4️⃣ Ridge & Lasso Regression

Regularised linear models

Reduce overfitting

Lasso performs feature selection

5️⃣ Polynomial Regression (Degree 2)

Captures smooth non-linear trends

Still interpretable

Avoids high-degree overfitting

6️⃣ Gradient Boosting Regressor (GBR)

Machine Learning model

Handles non-linear relationships

Robust to noise

Often strongest performer

No native uncertainty → estimated via residual dispersion.

🧮 Feature Engineering

Time is explicitly encoded using:

Feature	Purpose
t	Global trend
sin(2πt/12)	Annual seasonality
cos(2πt/12)	Annual seasonality

This allows ML models to correctly learn cyclical patterns.

📉 Uncertainty Estimation

Point forecasts alone are misleading.

Interval Methods
Model	Interval Source
SARIMA	Native confidence intervals
ML / Regression	±1.96 × RMSE

This approximates a 95% prediction interval.

Uncertainty bands:

Expand into the future

Reveal instability

Prevent false confidence

📊 Interactive Visualisation

Outputs are standalone interactive HTML files:

Toggle models on/off via legend

Hover for values

Zoom and pan

Compare uncertainty ranges visually

No Python or notebook required to view.

📁 Outputs

Each run generates:

One HTML forecast per selected crime

Timestamped for traceability

Example:

forecast_Robbery_20260107_153012.html

🧪 Interpretation Guidelines

Negative R² is not a bug
→ Model performs worse than predicting the mean

Exploding forecasts indicate instability
→ Often policy-driven or structurally changing crimes

Uncertainty bands matter more than point forecasts

🚧 What This Project Is / Is Not
✅ This IS:

Applied data science

Responsible forecasting

Interpretable ML

Decision-support analytics

❌ This is NOT:

Deep learning

Reinforcement learning

Real-time prediction

Causal inference

Those require significantly more data.

🚀 Future Extensions

Possible next steps:

Automatic model ranking

Ensemble forecasting

Structural break detection

Crime volatility classification

Spatial–temporal fusion

Policy impact simulation

Interactive dashboards (e.g. Streamlit)

📝 Final Note

This project follows professional forecasting standards:

Correct time-aware validation

Honest uncertainty

Multiple model comparison

Interpretability over hype

It is intentionally “heavy” because reality is heavy.

This README is intended to serve as:

Project anchor

Context reset for new AI chats

Foundation for future extensions
