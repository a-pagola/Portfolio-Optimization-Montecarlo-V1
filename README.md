# Portfolio Optimization with Markowitz & Monte Carlo (V1)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Status](https://img.shields.io/badge/Status-V1%20%7C%20V2%20in%20development-yellow)
![Domain](https://img.shields.io/badge/Domain-Quantitative%20Finance-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

An interactive Python notebook that applies **Modern Portfolio Theory (Markowitz)**
and a **Monte Carlo simulation** to approximate the **Efficient Frontier** and
optimize the weights of a 4-asset equity portfolio.

> **Note:** This repository contains **Version 1 (V1)** of the project. V1 uses a
> **heuristic approach** (random sampling via Monte Carlo) to *approximate* the
> efficient frontier. A mathematically exact solution using **convex optimization
> (`cvxpy`)** is currently in development as **Version 2 (V2)** — see the
> [Future Work / Roadmap](#future-work--roadmap) section.

---

## Table of Contents
- [Overview](#overview)
- [The Theory: Markowitz Mean-Variance Optimization](#the-theory-markowitz-mean-variance-optimization)
- [The Heuristic Approach: Monte Carlo Simulation](#the-heuristic-approach-monte-carlo-simulation)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Notebook Walkthrough](#notebook-walkthrough)
- [Results](#results)
- [Future Work / Roadmap](#future-work--roadmap)
- [About the Author](#about-the-author)
- [Disclaimer](#disclaimer)

---

## Overview

This project is the first step of my transition from **discretionary trading** to
**algorithmic / quantitative trading**. The goal is to build a reproducible,
data-driven framework to construct portfolios that maximize return for a given
level of risk — the core problem of **Modern Portfolio Theory**.

The notebook allows the user to:

1. Select **4 assets** from a curated universe of 26 US-listed tickers via an
   interactive dropdown interface (`ipywidgets`).
2. Download daily prices automatically with `yfinance`.
3. Visually inspect the individual performance of each asset.
4. Run a **Monte Carlo simulation** over thousands of random weight combinations.
5. Plot the **Efficient Frontier** and identify the **Maximum Sharpe Ratio** and
   **Minimum Variance** portfolios.

---

## The Theory: Markowitz Mean-Variance Optimization

Harry Markowitz (1952) formalized the idea that **risk and return cannot be
evaluated in isolation**. For a portfolio of $N$ assets with expected returns
$\mu$ and covariance matrix $\Sigma$:

- **Portfolio expected return:**
  $$\mu_p = \sum_{i=1}^{N} w_i \mu_i = w^T \mu$$

- **Portfolio variance (risk):**
  $$\sigma_p^2 = \sum_{i=1}^{N} \sum_{j=1}^{N} w_i w_j \sigma_{ij} = w^T \Sigma w$$

where $w$ is the vector of weights, subject to $\sum_i w_i = 1$ and (optionally)
$w_i \ge 0$ (no short-selling).

The **Efficient Frontier** is the set of portfolios that offer the **highest
expected return for a given level of risk** (or the lowest risk for a given
return). Any portfolio below the frontier is sub-optimal. The **tangency
portfolio** (Maximum Sharpe Ratio) is the point on the frontier that maximizes
risk-adjusted return:

$$ \text{Sharpe} = \frac{\mu_p - r_f}{\sigma_p} $$

The **Minimum Variance Portfolio** is the leftmost point of the frontier.

---

## The Heuristic Approach: Monte Carlo Simulation

Solving the optimization exactly requires a quadratic program (convex
optimization). **In V1, I deliberately took a simpler, heuristic route** to
build intuition and validate the data pipeline:

1. Generate **thousands of random weight vectors** $w$ (each summing to 1,
   non-negative).
2. For each vector, compute the **annualized return**, **annualized volatility**,
   and **Sharpe ratio** using historical data.
3. Scatter all portfolios in the risk/return plane.
4. The **upper-left envelope** of this cloud is a numerical approximation of the
   **Efficient Frontier**.
5. The portfolio with the **highest Sharpe ratio** and the one with the
   **lowest volatility** are highlighted.

**Trade-off:** this method is intuitive, easy to explain, and requires no
external solver — but it is **stochastic and approximate**. It will never find
the *exact* optimal weights, and results vary slightly between runs. This is
precisely the limitation that **V2** addresses.

---

## Features

- Interactive asset selection (4 dropdowns, 26 tickers).
- Automatic data download from Yahoo Finance (`yfinance`).
- Individual asset performance visualization.
- Monte Carlo simulation of the portfolio weight space.
- Efficient Frontier plot with Maximum Sharpe and Minimum Variance portfolios
  highlighted.
- Reproducible notebook, cleanly documented in Markdown.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Python 3.10+** | Core language |
| **pandas** | Data manipulation & time-series handling |
| **numpy** | Numerical computing & random weight generation |
| **yfinance** | Market data download |
| **matplotlib** | Plotting the Efficient Frontier |
| **seaborn** | Styling of statistical plots |
| **ipywidgets** | Interactive asset-selection interface |
| **Jupyter Notebook** | Reproducible research environment |

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/Portfolio-Optimization-Markowitz.git
cd Portfolio-Optimization-Markowitz