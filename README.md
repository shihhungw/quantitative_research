# Replicating Foreign Securities Firms' Trading Performance in the Taiwan Stock Market

This project explores whether we can quantitatively replicate the trading performance of five foreign securities firms operating in Taiwan. By analyzing their trading patterns and applying machine learning models, we develop a simple yet effective strategy that mirrors their cumulative returns based on prior-day trading behavior—particularly for stocks exhibiting negative kurtosis.

## 🔍 Motivation

Several foreign brokerage firms in Taiwan consistently outperform the market. Although we don't have access to their internal strategies or information, their trading records are publicly available. If these firms' success is persistent, can we reverse-engineer their decisions and replicate their results using only historical data?

## 🧠 Models & Strategy

The project follows a two-stage approach:

### 1. **Forecasting Trading Behavior**
- Convert firm-level trading volume into real-valued net positions
- Define large trades using 2× standard deviation threshold
- Use the following models to predict next-day trading:
  - **OLS Regression**: Predict trading amount (continuous)
  - **Logistic Regression**: Predict probability of large net buying/selling
  - **Random Forest**: Nonlinear model for direction & amount

### 2. **Return Replication Strategy**
- **Key insight**: When the stock's price distribution shows **negative kurtosis**, mirroring the firm’s previous day trading volume effectively replicates cumulative returns
- Backtest shows **>80% replication accuracy** in terms of direction and magnitude of returns

## 🧩 Data

We used the following daily-level datasets:

- **Securities Firms**: Net buying/selling volume, transaction prices
- **Stocks**: OHLC prices, trading volume
- **Indices**: TWSE index, Taiwan 50, industry sector indices
- **Macro Indicators**: GDP, unemployment rate, interest rate, CPI, PMI

Data were manually cleaned and aligned across trading days to ensure consistency.

## 📈 Results

| Model              | Prediction Goal      | Key Finding                                |
|-------------------|----------------------|--------------------------------------------|
| OLS Regression     | Net Trading Volume   | Weak statistical significance              |
| Logistic Regression | Large Trade Flag     | Predictive above 70% in limited cases      |
| Random Forest      | Volume & Direction   | Stronger for classification than regression |
| **Replication Strategy** | Cumulative Returns | >80% success rate for negative-kurtosis stocks |

This highlights that **price distribution shape** (specifically kurtosis) can be a critical filtering condition in designing copy-trading strategies.

## 💡 Reflection

This project bridges quantitative modeling and market intuition. Rather than relying solely on prediction accuracy, we used stylized facts (e.g. kurtosis) to guide actionable strategies. It also shows how simple rules based on public data can achieve meaningful financial replication—even without access to proprietary models.

## 📌 Requirements

- Python 3.7+
- `pandas`, `numpy`, `scikit-learn`
- `matplotlib`, `seaborn` (for visualization)

Install via:

```bash
pip install -r requirements.txt


.
├── Data Preprocessing.ipynb       # Cleaning, transformation, labeling of raw data
├── Model Analysis - Logit_RF_XGBoost.ipynb  # Main ML model training and evaluation
├── Backtesting.ipynb              # Trading rule and return replication logic
├── Model Analysis.py              # Script version for visualization comparison
├── README.md                      # Project overview
