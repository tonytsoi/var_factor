# Overview of Risk Factor Models and Its Application in VaR Estimation

Read the full article on <a href="https://medium.com/@tsoiyingkit/overview-of-risk-factor-model-and-its-application-in-var-estimation-8eff11f90bef">Medium</a>.

## Motivation

As the number of assets in a portfolio grows, reliably estimating a covariance matrix requires a proportionally large number of historical observations. This becomes especially problematic when some stocks have only recently started trading and lack sufficient price history.

Risk factor models address this by reducing the dimensionality of the problem: instead of estimating pairwise covariances across all assets, we estimate each asset's exposure to a small number of common risk factors, then derive the covariance structure from those factors. This produces more stable estimates and also surfaces the underlying sources of systematic risk driving the portfolio.

## Theoretical Background

The notebook draws on three foundational theories:

- **Modern Portfolio Theory (MPT)** — the basis for mean-variance optimisation and the role of diversification
- **Capital Asset Pricing Model (CAPM)** — introduces the market as a single risk factor explaining expected returns
- **Arbitrage Pricing Theory (APT)** — generalises CAPM to allow multiple systematic risk factors, providing the theoretical grounding for multi-factor regression models

## What the Notebook Covers

### 1. Fama-French Risk Factor Model

A fundamental factor model that constructs risk factors directly from observable stock characteristics.

- Stocks are sorted into 6 subportfolios along two dimensions:
  - **Size**: split at the median market capitalisation (Big / Small)
  - **Value**: split at the 30th and 70th percentiles of Price-to-Book ratio (High / Neutral / Low)
- Factor returns are derived as:
  - **Value (HML)** = average return of high P/B stocks − average return of low P/B stocks
  - **Size (SMB)** = average return of small-cap stocks − average return of large-cap stocks
  - **Market** = S&P 500 daily return
- A time-series regression of each stock's returns against these factors yields its factor exposures (betas)

**Example — Apple (AAPL):**

| Factor | Beta |
|--------|------|
| Market | 1.04 |
| Size   | −0.23 |
| Value  | 0.16 |

### 2. Barra Risk Factor Model

An alternative fundamental approach that uses cross-sectional regressions rather than constructing factor portfolios explicitly.

- Standardise the stock characteristics (Market Cap and P/B ratio) at each point in time to form factor exposures
- Run a cross-sectional regression across stocks at each date to back out implied factor returns for that day
- Run a time-series regression of each stock's returns against the estimated factor return series to obtain betas

**Example — Apple (AAPL):**

| Factor | Beta |
|--------|------|
| Market | 1.23 |
| Size   | −0.34 |
| Value  | −0.27 |

The notebook also produces comparison scatter plots showing how the Barra-estimated size and value factor return series align with the Fama-French equivalents.

### 3. Statistical Factor Model (PCA)

Rather than constructing factors from pre-specified characteristics, Principal Component Analysis (PCA) discovers factors statistically by finding orthogonal directions of maximum variance.

**Motivating example:** Two highly correlated simulated return series (ρ ≈ 1) are decorrelated by PCA, demonstrating how the technique extracts independent factors from correlated data.

**Real-world application — US Treasury Yield Curve:**

PCA is applied to daily US Treasury yields across 10 maturities (3M to 30Y). The three principal components have intuitive economic interpretations:

| Component | Interpretation | Description |
|-----------|---------------|-------------|
| PC1 | Level | Parallel shifts — all yields move up or down together |
| PC2 | Slope | Steepness — short and long rates diverge |
| PC3 | Curve | Curvature — the middle of the yield curve moves relative to the wings |

The notebook shows both the time series of each component and the maturity loadings (eigenvectors) that define them.

![ff_fac](https://github.com/tonytsoi/var_factor/blob/main/ff_fac.png?raw=true)

### 4. VaR and Expected Shortfall — Parametric Method

The notebook integrates the factor framework into a practical VaR calculation for an equal-weighted portfolio of 12 large-cap stocks.

**Method:**

1. Regress each stock's returns against the Fama-French 5-factor model to obtain individual factor exposures
2. Aggregate to portfolio-level factor exposures: **w_f = B' · w** (where w are the portfolio weights)
3. Compute portfolio volatility using the factor covariance matrix **Σ**:

$$\sigma_p = \sqrt{\mathbf{w}_f^\top \Sigma \, \mathbf{w}_f}$$

4. Apply the normal distribution to obtain VaR and Expected Shortfall at the 95% confidence level:

$$\text{VaR}_{0.95} = \mu_p + z_{0.05} \cdot \sigma_p$$

$$\text{ES}_{0.95} = \mu_p - \frac{\phi(z_{0.05})}{0.05} \cdot \sigma_p$$

**Results:**

| Metric | Value |
|--------|-------|
| 95% VaR | −2.31% |
| 95% Expected Shortfall | −2.92% |

## Data Sources

| Dataset | Source |
|---------|--------|
| Stock prices and fundamentals (Market Cap, Stockholders' Equity) | `yfinance` |
| US Treasury daily yield curve rates | `daily-treasury-rates.csv` |
| Fama-French 5-Factor daily data | `F-F_Research_Data_5_Factors_2x3_daily.csv` (Kenneth French's website) |

## Portfolio Stocks

The notebook uses an equal-weighted portfolio of 12 large-cap US stocks:

NVDA, AAPL, MSFT, AMZN, GOOG, META, TSLA, AVGO, BRK-B, WMT, JPM, LLY

## Requirements

```
yfinance
pandas
numpy
scikit-learn
statsmodels
seaborn
matplotlib
```

Run all cells in `Risk Factor Model.ipynb` top-to-bottom. Stock and index data are downloaded automatically via `yfinance`; the Treasury rates and Fama-French CSVs must be present in the working directory.
