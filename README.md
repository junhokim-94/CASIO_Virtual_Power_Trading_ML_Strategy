# CAISO Virtual Trading Strategy with ML Forecasting (SVR, XGBoost, Random Forest)

## Project Overview

This project develops and evaluates multiple machine learning-based trading strategies for **virtual bidding in the California Independent System Operator (CAISO)** electricity market. The objective is to forecast Day-Ahead Market (DAM) and Real-Time Market (RTM) prices and exploit their spread for arbitrage profits using predictive models. We explore XGBoost, Support Vector Regression (SVR), Random Forest, and baseline Linear Regression.

## Motivation

- **Systematic DAM–RTM divergence** arises due to forecast errors, ramping constraints, and risk premia.
- **Trader behavior can be reverse-engineered** using historical virtual bidding patterns.
- **ML models outperform traditional models** in capturing non-linear, spiky price behaviors.

## Market Context

- CAISO is a $40B market managing power supply for 40M people.
- Virtual trading enables arbitrage between DAM and RTM without physical delivery.
- Three main trading hubs: NP15, SP15, ZP26.

## Trading Strategy

### Profit Logic
| Direction | Forecast Conditions                | Execution Conditions             | Profit Calculation            |
|-----------|------------------------------------|----------------------------------|-------------------------------|
| Long      | RTM > DAM + 0.5                    | Forecast DAM ≥ Actual DAM        | Actual RTM − Actual DAM       |
| Short     | RTM < DAM - 0.5                    | Forecast DAM < Actual DAM        | Actual DAM − Actual RTM       |

### Input Features
- DAM and RTM historical prices (lagged)
- Forecasted solar, wind, gas generation
- Total system load, net imports
- Rolling averages, lagged variables (24h, 48h)
- Built per hub and per hour

## Models & Performance

### Linear Regression
- ROI: 0.57%, Win Rate: 47.8%
- Poor fit (R² ~0.2–0.3)

### XGBoost
- ROI: 0.73%, Profit: $3.8k on $520k invested
- Improves DAM forecasts, minor accuracy gain

### Random Forest
- ROI: 1.54%, Profit: $6.0k on $390k invested
- Best predictive accuracy (R² = 0.78 for DAM)

### Support Vector Regression (SVR)
- ROI: **9.61%**, Profit: **$29.7k** on $309k invested
- High directional accuracy (63.5% win rate)
- Exploits structural bias where DAM > RTM ~65% of time

## Key Insight

SVR accidentally overpredicts both DAM and RTM, but inflates DAM more, generating short signals that align with structural market inefficiency. Tree models (e.g., XGBoost, RF) overpredict RTM more, leading to less profitable long signals.

## Limitations & Next Steps

- Low overall ROI; below risk-free rate when costs included
- SVR’s success may stem from accidental alignment with DAM–RTM spread structure
- Improvements:
  - Walk-forward validation
  - Node-level pricing
  - Incorporate volume, slippage, constraints
  - Causal feature inclusion or ensemble models

## How to Run

1. Install dependencies:
```bash
pip install pandas matplotlib seaborn statsmodels xgboost scikit-learn pyarrow
```

2. Run:
```bash
python "CAISO VIRTUAL TRADING STRATEGY_SVR.py"
```

## Authors

- Junho Kim
- Ton That, Hoa
- Quillet, Emillie
- MacKenzie, Kai
- Chen, Xue Song