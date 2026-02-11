# Time-Series Forecasting for Financial Balance Risk
**MIT Global Challenge — Time Series Pipeline**

This repository implements a **production-oriented time-series forecasting pipeline** for financial balance prediction, developed as part of the **MIT Global Challenge**.

The project reframes classical forecasting as a **supervised learning problem**, combining **XGBoost**, **Prophet**, and **probabilistic Monte Carlo simulation** to produce both point forecasts and full risk-aware predictive distributions.

---

## Problem Setting

Traditional statistical models (e.g. ARIMA) often struggle with:
- Non-stationarity
- High noise
- Regime shifts
- Limited historical data

This work addresses these limitations by:
- Converting time series into supervised tabular features
- Leveraging gradient-boosted trees for non-linear dynamics
- Explicitly modelling forecast uncertainty and downside risk

---

## Approach Overview

### 1. Data Aggregation & Preprocessing
- Daily aggregation of balances, credits, and debits
- Missing values handled deterministically (filled with zero)
- Clean, time-indexed dataset suitable for sequential inference

### 2. Feature Engineering
**Calendar & regime features**
- Day of week, month, quarter, week of year
- Weekend and month boundary indicators
- Seasonal regime bins

**Transaction dynamics**
- Daily credits, debits, net cash flow
- Lagged transaction features (1, 3, 7 days)

**Lagged target features**
- Balance lags from 1–14 days

**Rolling statistics**
- Shifted rolling mean, std, min, max
- Windows: 3, 7, 14, 30 days
- Explicit leakage prevention via temporal shifting

---

## Models

### XGBoost
- Gradient-boosted decision tree regressor
- Learns non-linear interactions between lagged, rolling, and calendar features
- Trained using strict chronological splits

### Prophet (baseline)
- Used as a structural benchmark
- Captures trend + seasonality under strong modelling assumptions

---

## Evaluation & Validation

### Chronological Train–Test Split
- Training: 175 days (Dec 2024 – Aug 2025)
- Test: 44 days (Aug 2025 – Oct 2025)
- No randomization or look-ahead bias

### Time-Series Cross-Validation
- Expanding-window validation
- Fixed forward test horizon
- Evaluated across three folds

**Cross-validation summary**
- RMSE (mean): ~$128k  
- MAE (mean): ~$99k  
- High variance across folds reflects limited data and regime sensitivity

This variability highlights a core insight: **feature-rich models benefit significantly from longer historical coverage**, especially in financial settings.

---

## Probabilistic Forecasting via Monte Carlo Simulation

To move beyond point estimates:
- Historical forecast residuals are modeled as a parametric distribution
- 1,000 Monte Carlo paths are sampled and added to the point forecast
- Produces a full predictive distribution over future balances

This enables:
- Uncertainty quantification
- Tail-risk analysis
- Probability-of-breach estimates relative to risk thresholds

Rather than a single forecast number, the model outputs a **distribution of plausible futures**, supporting risk-aware decision-making.

---

## Key Outcomes

- Demonstrated that **XGBoost-based supervised forecasting** is more stable than classical models under noisy, non-stationary conditions
- Built a **leakage-safe, feature-rich time-series pipeline**
- Extended deterministic forecasts into **probabilistic risk distributions**
- Quantified downside risk and forecast uncertainty explicitly

---

## Limitations & Future Work

- Dataset size limits cross-validation stability
- Performance variance reflects sensitivity to regime shifts
- Future work includes:
  - Longer historical windows
  - Hierarchical / multivariate extensions
  - Bayesian residual modelling
  - Online updating and adaptive retraining

---

## Status

Research prototype developed for the MIT Global Challenge.  
Designed as a foundation for further work in:
- Financial time-series forecasting
- Quantitative risk modelling
- Probabilistic ML systems
  
## Demonstration & Dissemination

This work was **publicly demonstrated** at the **BlendED AI+X Global Meetup** in Singapore, hosted at the **:contentReference[oaicite:0]{index=0} (NUS)**.

The live demonstration showcased:
- End-to-end time-series forecasting using XGBoost and Prophet  
- Leakage-safe feature engineering for financial sequences  
- Probabilistic forecasting via Monte Carlo simulation  
- Risk-aware interpretation of forecast distributions under limited data

The session engaged a multidisciplinary audience spanning AI research, industry practitioners, and founders, providing feedback on model interpretability, robustness, and real-world deployment considerations.
