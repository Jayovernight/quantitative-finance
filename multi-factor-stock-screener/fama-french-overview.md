# Fama-French Factor Models: A Scientific Overview

> A concise reference on the academic foundations of factor investing,
> from the original three-factor model to modern multi-factor portfolio construction.

---

## Table of Contents

1. [The Capital Asset Pricing Model (CAPM) Baseline](#1-the-capital-asset-pricing-model-capm-baseline)
2. [Fama-French 3-Factor Model (1993)](#2-fama-french-3-factor-model-1993)
3. [Carhart 4-Factor Model (1997)](#3-carhart-4-factor-model-1997)
4. [Fama-French 5-Factor Model (2015)](#4-fama-french-5-factor-model-2015)
5. [Factor Definitions](#5-factor-definitions)
6. [Empirical Evidence](#6-empirical-evidence)
7. [Factor Investing in Practice](#7-factor-investing-in-practice)
8. [Connection to Quantitative Portfolio Construction](#8-connection-to-quantitative-portfolio-construction)
9. [Key References and Data Sources](#9-key-references-and-data-sources)

---

## 1. The Capital Asset Pricing Model (CAPM) Baseline

Sharpe (1964) and Lintner (1965) proposed that the expected excess return of any
asset is proportional to its market beta:

```
E[Ri] - Rf = Bi * (E[Rm] - Rf)
```

where `Ri` is the asset return, `Rf` the risk-free rate, `Rm` the market return,
and `Bi` the asset's sensitivity to the market factor.

CAPM provides the single-factor benchmark against which all subsequent multi-factor
models are measured. Its well-documented inability to explain cross-sectional return
dispersion motivated the search for additional systematic risk factors.

---

## 2. Fama-French 3-Factor Model (1993)

Fama and French (1993) augmented CAPM with two factors capturing the historical
outperformance of small-cap stocks and high book-to-market (value) stocks:

```
Ri - Rf = ai + Bi*(Rm - Rf) + si*SMB + hi*HML + ei
```

| Factor | Long Leg | Short Leg | Captures |
|--------|----------|-----------|----------|
| **Rm - Rf** | Market portfolio | Risk-free rate | Equity risk premium |
| **SMB** | Small-cap stocks | Big-cap stocks | Size premium |
| **HML** | High B/M stocks | Low B/M stocks | Value premium |

The intercept `ai` (alpha) represents the portion of return unexplained by the
three factors. A statistically significant alpha indicates either mispricing or
exposure to omitted risk factors.

### Construction Methodology

Fama and French sort stocks independently on size (market capitalization, median
NYSE breakpoint) and book-to-market (30th/70th percentile NYSE breakpoints),
forming six value-weighted portfolios (2x3 sort). SMB and HML are then computed
as the appropriate averages of portfolio return differences.

---

## 3. Carhart 4-Factor Model (1997)

Jegadeesh and Titman (1993) documented that stocks with strong recent performance
continue to outperform over horizons of 3 to 12 months. Carhart (1997) added a
momentum factor to the Fama-French framework:

```
Ri - Rf = ai + Bi*(Rm - Rf) + si*SMB + hi*HML + wi*WML + ei
```

| Factor | Long Leg | Short Leg | Lookback |
|--------|----------|-----------|----------|
| **WML** (Winners Minus Losers) | Top 30% past 12-1 month return | Bottom 30% | 12 months, skip most recent month |

The one-month skip avoids the well-documented short-term reversal effect.
WML is sometimes labeled UMD (Up Minus Down) in the literature.

### Why Momentum is Not in Fama-French 5

Fama and French (2015) deliberately excluded momentum from their five-factor model,
arguing it is a "premier anomaly" that their valuation framework does not explain.
Practitioners often combine the five factors with momentum as a sixth factor.

---

## 4. Fama-French 5-Factor Model (2015)

Fama and French (2015) introduced two additional factors motivated by the dividend
discount model identity linking expected returns to profitability and investment:

```
Ri - Rf = ai + Bi*(Rm - Rf) + si*SMB + hi*HML + ri*RMW + ci*CMA + ei
```

| Factor | Long Leg | Short Leg | Captures |
|--------|----------|-----------|----------|
| **RMW** | Robust profitability | Weak profitability | Profitability premium |
| **CMA** | Conservative investment | Aggressive investment | Investment premium |

The five-factor model substantially reduces the alpha of portfolios sorted on
profitability and investment, which were left unexplained by the three-factor model.

A notable finding: **HML becomes redundant** (its alpha is close to zero) once RMW
and CMA are included, because value is largely captured by the interaction of
profitability and investment. However, Fama and French retain HML for continuity.

---

## 5. Factor Definitions

### Market Excess Return (Rm - Rf)

```
Rm - Rf = Return on the value-weighted market portfolio - Treasury bill rate
```

### SMB (Small Minus Big)

```
SMB = (1/3) * (Small Value + Small Neutral + Small Growth)
    - (1/3) * (Big Value   + Big Neutral   + Big Growth)
```

Computed from the 2x3 size/B-M sort. In the five-factor model, SMB is averaged
across three independent 2x3 sorts (size/B-M, size/OP, size/Inv).

### HML (High Minus Low)

```
HML = (1/2) * (Small Value + Big Value) - (1/2) * (Small Growth + Big Growth)
```

Book-to-market ratio = Book equity / Market equity. Annual rebalancing in June,
using book equity from the fiscal year ending in the prior calendar year (t-1).

### WML (Winners Minus Losers)

```
WML = (1/2) * (Small Winners + Big Winners) - (1/2) * (Small Losers + Big Losers)
```

Formed on cumulative return from month t-12 to t-2. Monthly rebalancing.

### RMW (Robust Minus Weak)

```
RMW = (1/2) * (Small Robust + Big Robust) - (1/2) * (Small Weak + Big Weak)
```

Operating profitability = (Revenue - COGS - SGA - Interest) / Book equity.

### CMA (Conservative Minus Aggressive)

```
CMA = (1/2) * (Small Conservative + Big Conservative)
    - (1/2) * (Small Aggressive   + Big Aggressive)
```

Investment = Growth in total assets from fiscal year t-2 to t-1.

---

## 6. Empirical Evidence

### Long-Run Premia (US, 1963-2023, annualized)

| Factor | Average Premium | t-statistic | Sharpe Ratio |
|--------|---------------:|------------:|-------------:|
| Rm - Rf | ~8.0% | >4.0 | ~0.40 |
| SMB | ~2.0% | ~2.0 | ~0.15 |
| HML | ~3.5% | ~2.5 | ~0.25 |
| WML | ~6.5% | ~3.5 | ~0.45 |
| RMW | ~3.0% | ~3.0 | ~0.30 |
| CMA | ~2.5% | ~2.0 | ~0.20 |

*Approximate figures from Kenneth French Data Library. Exact values depend on
the sample period.*

### Key Findings

- **Value (HML)** has been persistent over 90+ years in US data (Davis, Fama, and
  French, 2000) but experienced a prolonged drawdown from 2007 to 2020.
- **Momentum (WML)** delivers the highest Sharpe ratio among individual factors
  but suffers from occasional severe crashes (e.g., -73% in Q2 2009).
- **Profitability (RMW)** and **investment (CMA)** are more robust out-of-sample
  and show less cyclicality than value.
- **Size (SMB)** is the weakest and most debated factor. Much of its premium
  concentrates in micro-caps and January.

### International Evidence

Fama and French (2012, 2017) confirmed value, momentum, and profitability premia
in international markets (Europe, Japan, Asia-Pacific), though magnitudes vary:

- Value tends to be **stronger** in Japan and Europe than in the US.
- Momentum is **weaker or absent** in Japan but strong in Europe.
- Size is **inconsistent** across regions.
- Profitability is **globally robust**, especially in developed markets.

### Factor Decay and Crowding

McLean and Pontiff (2016) found that published anomalies decay by roughly 30%
post-publication, raising important questions about the persistence of factor
premia in a world where institutional capital actively harvests them.

---

## 7. Factor Investing in Practice

### Smart Beta and Factor ETFs

The ETF industry has made factor exposure accessible through products tracking
single-factor and multi-factor indices. Major index providers (MSCI, FTSE Russell,
S&P DJI) maintain factor index families with transparent methodologies.

### Multi-Factor Approaches

Practitioners typically combine factors using one of two methods:

1. **Portfolio-level mixing** -- Allocate across single-factor portfolios. Simple
   to implement but dilutes factor exposure and may hold offsetting positions.

2. **Stock-level integration** -- Score each stock on multiple factors
   simultaneously and select those with the best composite score. Produces
   more concentrated factor exposure and avoids internal hedging.

The academic consensus (Ghayur, Hatchett, and Trexotic, 2019; Fitzgibbons et al.,
2017) favors stock-level integration for higher capital efficiency.

### Rebalancing and Transaction Costs

Factor strategies require periodic rebalancing to maintain target exposures.
Higher-turnover factors (momentum) incur greater transaction costs, which can
erode a significant portion of gross returns. Buffer rules, partial rebalancing,
and turnover constraints are standard mitigations.

### Risk Management

Factor investing does not eliminate drawdown risk. Value and momentum are
negatively correlated in the short run but can both suffer during market
dislocations. Robust implementations monitor factor crowding, liquidity
conditions, and tail-risk exposures.

---

## 8. Connection to Quantitative Portfolio Construction

Factor models serve two distinct roles in quantitative finance:

1. **Return prediction** -- Composite factor scores (value + momentum +
   profitability + ...) rank stocks by expected forward return. The mapping
   from factor scores to portfolio weights is the core of systematic equity
   selection.

2. **Risk decomposition** -- Factor covariance matrices (e.g., Barra, Axioma)
   decompose portfolio risk into systematic factor contributions and
   idiosyncratic residuals, enabling risk-aware optimization.

A typical quantitative workflow:

```
Raw data --> Factor score computation --> Composite ranking
         --> Optimization (maximize composite score subject to risk/turnover constraints)
         --> Trade execution --> Performance attribution (factor vs. alpha)
```

The separation of **alpha signal** (factor scores) and **portfolio construction**
(optimization) is a fundamental principle. The same set of factor views can
produce very different portfolios depending on the optimizer's constraints
on concentration, sector exposure, turnover, and tracking error.

---

## 9. Key References and Data Sources

### Foundational Papers

- Sharpe, W. F. (1964). Capital Asset Prices. *Journal of Finance*, 19(3).
- Fama, E. F., & French, K. R. (1993). Common Risk Factors in the Returns on
  Stocks and Bonds. *Journal of Financial Economics*, 33(1), 3-56.
- Carhart, M. M. (1997). On Persistence in Mutual Fund Performance.
  *Journal of Finance*, 52(1), 57-82.
- Jegadeesh, N., & Titman, S. (1993). Returns to Buying Winners and Selling
  Losers. *Journal of Finance*, 48(1), 65-91.
- Fama, E. F., & French, K. R. (2015). A Five-Factor Asset Pricing Model.
  *Journal of Financial Economics*, 116(1), 1-22.
- Fama, E. F., & French, K. R. (2017). International Tests of a Five-Factor
  Asset Pricing Model. *Journal of Financial Economics*, 123(3), 441-463.

### Further Reading

- Asness, C. S., Moskowitz, T. J., & Pedersen, L. H. (2013). Value and Momentum
  Everywhere. *Journal of Finance*, 68(3), 929-985.
- Novy-Marx, R. (2013). The Other Side of Value: The Gross Profitability Premium.
  *Journal of Financial Economics*, 108(1), 1-28.
- McLean, R. D., & Pontiff, J. (2016). Does Academic Research Destroy Stock
  Return Predictability? *Journal of Finance*, 71(1), 5-32.
- Harvey, C. R., Liu, Y., & Zhu, H. (2016). ... and the Cross-Section of
  Expected Returns. *Review of Financial Studies*, 29(1), 5-68.

### Data Sources

- **Kenneth French Data Library**: [https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html)
  -- Free monthly and daily factor returns for US, developed, and emerging markets.
- **AQR Data Sets**: [https://www.aqr.com/Insights/Datasets](https://www.aqr.com/Insights/Datasets)
  -- Momentum, value, and other factor data across asset classes.
- **WRDS (Wharton Research Data Services)**: [https://wrds-www.wharton.upenn.edu/](https://wrds-www.wharton.upenn.edu/)
  -- CRSP, Compustat, and other databases used in academic factor research.

---

*This document provides a scientific overview of factor investing theory.
It does not constitute investment advice or describe any specific trading system.*
