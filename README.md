# 📈 DMA200 + EMA50 Slope Crossover Portfolio Backtester

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

A capital-efficient, multi-asset backtesting framework for evaluating a **DMA200 slope + EMA50 slope crossover trend-following strategy** across Indian equities (NIFTY 50 / NIFTY Next 50).

This project emphasizes:

* ✅ Realistic capital allocation
* ✅ Portfolio-level concurrency modeling
* ✅ Capital-adjusted CAGR
* ✅ Risk-aware performance metrics
* ✅ Parameter optimization with train/test splits

---

# 🧠 Strategy Overview

## Indicators Used

* **200 DMA Slope** → Long-term trend direction
* **50 EMA Slope** → Medium-term momentum confirmation
* Optional **Bollinger-based execution filter**

## Entry Conditions

1:
* DMA200 slope < 0
* EMA50 slope > 0
* Optional price validation within candle range

2:
* DMA200 slope > 0
* indicators
* Optional price validation within candle range

## Exit Conditions

* ATR based trailing stop loss

---

# 🔄 Backtesting Workflow

## 1️⃣ Data Download & Preprocessing

```python
df = download_data(stock)
df = modifications(df)
```

## 2️⃣ Strategy Execution

```python
equity = backtest_strategy(
    df,
    slope50_window,
    slope200_window,
    bb_price
)
```

## 3️⃣ Performance Metrics

```python
cagr = calculate_cagr(equity)
max_dd = calculate_max_drawdown(equity)
sortino = calculate_sortino(returns)
```

---

# 📊 Performance Metrics

## CAGR

Annualized growth rate calculated from the equity curve time span.

## Max Drawdown

```python
rolling_max = equity_curve.cummax()
drawdown = (equity_curve - rolling_max) / rolling_max
max_dd = abs(drawdown.min())
```

## Sortino Ratio (India Risk-Free Rate)

Annual RF default: **6%**

```python
rf_daily = (1 + 0.06) ** (1/252) - 1
```

---

# 💰 Capital-Efficient Portfolio Modeling

Traditional backtests assume full capital deployment at all times.

This framework instead:

1. Collects all trades across stocks
2. Builds a unified trade table
3. Computes daily open positions
4. Measures concurrency

## Key Metrics

* **Max Concurrent Positions**
* **Average Concurrent Positions**
* **Capital Utilization**

## Required Portfolio Capital

```
Required Capital = Max Concurrent Positions × Capital Per Trade
```

---

# 📈 Practical CAGR Adjustment

Let:

* `c` = raw CAGR
* `N` = total stocks tested
* `C_eff` = operational concurrent positions

```
Practical CAGR = (1 + c)^(N / C_eff) - 1
```

Recommended:

```
C_eff = 70–80% of Max Concurrent Positions
```

This ensures:

* Realistic deployability
* Capital buffer for overlapping trades
* Drawdown absorption

---

# 🧪 Parameter Optimization

* Grid search over slope windows
* Random train/test stock split
* Ranking via composite score
* Disqualification handling

Example:

```python
param_grid = {
    "slope50_window": [3, 5, 7, 10],
    "slope200_window": [7],
    "bb_price": ["BB_LM0.01"]
}
```

# 📌 Example Output (Illustrative)

---

# ⚠️ Assumptions

* No slippage or brokerage (can be added)
* Fixed capital per trade
* Daily frequency signals
* No leverage unless explicitly modeled

---

# 📜 Disclaimer

This project is for research and educational purposes only. Past performance does not guarantee future returns.

---

⭐ If you found this useful, consider starring the repository.
