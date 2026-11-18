# Monte Carlo Portfolio Optimization

Monte Carlo simulation applied to mean-variance portfolio optimization under **Modern Portfolio Theory (Markowitz, 1952)**. Given a set of assets, the model runs thousands of random weight combinations to approximate the efficient frontier and identify the portfolio that maximizes the **Sharpe Ratio**.

![Python](https://img.shields.io/badge/Python-3.10-blue) ![License](https://img.shields.io/badge/license-MIT-green)

---

## Methodology

1. **Data** — Historical daily closing prices downloaded via `yfinance` (configurable date range).
2. **Log returns** — Daily log-returns computed as $r_t = \ln(P_t / P_{t-1})$ to ensure stationarity and time-additivity.
3. **Monte Carlo simulation** — 5,000 random weight vectors are sampled from a uniform Dirichlet distribution, normalized so weights sum to 1 (long-only constraint).
4. **Portfolio metrics** — For each weight vector, annualized return, volatility and Sharpe Ratio are computed using the covariance matrix of log-returns scaled by 252 trading days.
5. **Efficient frontier** — Approximated by selecting the maximum-return portfolio within each volatility bucket across the simulated space.

$$\text{Sharpe} = \frac{R_p - R_f}{\sigma_p}, \quad R_f = 2\%$$

---

## Example Results

Portfolio: **AAPL · NVDA · SPY · GLD** — Period: 2015–2024

| Metric | Value |
|---|---|
| Annualized Return | ~24.8% |
| Annualized Volatility | ~18.27% |
| Sharpe Ratio | ~1.25 |
| Optimal weights | NVDA 10.95% · AAPL 28.63% · SPY 0.02% · GLD 66.4% |

> Results are in-sample and should not be interpreted as forward-looking performance.

---

## Installation

```bash
git clone https://github.com/your-a-pagola/portfolio-optimization-motecarlo-V1.git
cd portfolio-optimization-V1
pip install -r requirements.txt
jupyter lab
```

**Dependencies:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `yfinance`, `ipywidgets`

---

## Usage

Run cells in order. In the first interactive cell, select 4 assets from the dropdown menu (26 available: Apple, NVIDIA, Tesla, S&P 500 ETF...) and click **Descargar datos** to fetch price data automatically.

---

## Known Limitations

- **In-sample optimization only.** Weights are optimized over the full historical period, which introduces look-ahead bias. A walk-forward or train/test split would be needed to assess out-of-sample robustness.
- **Monte Carlo approximation.** Random sampling does not guarantee finding the true efficient frontier. Analytical optimization via `scipy.optimize` or `cvxpy` would produce exact results.
- **Static allocation.** The model finds a fixed weight vector; it does not account for changing correlations or regime shifts over time.
- **Normality assumption.** The Sharpe Ratio assumes normally distributed returns. Financial returns exhibit fat tails and skewness, which this model does not capture.
- **Transaction costs not modeled.** Real rebalancing involves costs that would reduce the effective Sharpe Ratio.

---

## Potential Extensions

- Analytical efficient frontier with `scipy.optimize`
- Out-of-sample validation with walk-forward analysis
- Sortino Ratio and Maximum Drawdown as additional risk metrics
- Dynamic rebalancing strategy with periodic weight updates

---

## Context

This project is part of a broader learning path toward quantitative finance and systematic trading. It covers foundational concepts in portfolio theory that are directly relevant to risk management and strategy development in institutional settings.