# Beyond Macro Fundamentals

## Informed Trading, Systemic Risk and Sovereign Contagion in CDS Spreads

This repository contains the code, empirical results and final report of an econometrics project investigating the information embedded in Credit Default Swap spreads.

The analysis studies whether CDS spread changes contain a latent component that is not explained by observable macroeconomic and firm-specific variables, whether CDS markets anticipate firm-level credit deterioration, and whether banking-sector credit risk propagates to sovereign CDS markets.

The project combines panel-data econometrics, event-study techniques, Principal Component Analysis, Random Matrix Theory and time-series models.

---

## Research Questions

The project addresses three main questions:

1. **Do CDS spreads anticipate firm-level credit deterioration?**

   Large positive changes in firm leverage are used as proxy credit events. CDS and equity movements around these events are examined to determine whether CDS markets behave as early-warning sensors or mainly react after deterioration becomes observable.

2. **Do bank CDS residuals contain a statistically significant common factor?**

   Principal Component Analysis is applied to the residuals of a panel regression. Random Matrix Theory is then used to distinguish statistically meaningful components from noise.

3. **Does European banking-sector credit risk predict sovereign CDS movements?**

   A Vector Autoregression links a latent European banking factor to changes in Spanish and Portuguese sovereign CDS spreads, allowing the analysis of Granger causality and short-run impulse responses.

---

## Data

The main dataset is an unbalanced panel of daily five-year CDS spreads covering the period from **January 2015 to September 2021**.

After data cleaning and filtering, the final corporate panel contains:

| Quantity | Value |
|---|---:|
| Firms | 279 |
| Firm-day observations | 464,383 |
| Minimum observations per firm | 1,400 |
| CDS tenor | 5 years |
| Sample period | January 2015 – September 2021 |

The CDS data are combined with:

- firm leverage;
- equity returns;
- the VIX index;
- the three-month US Treasury yield;
- the US yield-curve slope, defined as the ten-year minus two-year yield.

The main data sources are:

- Markit/Datastream CDS data;
- Refinitiv firm-level leverage data;
- FRED macro-financial data;
- Yahoo Finance and Investing.com equity-market data.

A separate banking subsample contains **20 Global Systemically Important Banks**, consisting of:

- 7 US banks;
- 1 Canadian bank;
- 10 European banks;
- 2 Japanese banks.

Due to licensing restrictions, proprietary raw data are not redistributed in this repository.

---

## Empirical Methodology

### 1. Panel Regression and Latent-Factor Identification

The analysis begins by testing the stationarity of CDS spread levels and first differences using firm-level ADF-GLS tests.

Since the first differences are stationary, the dependent variable is the daily change in the five-year CDS spread.

The benchmark specification includes changes in:

- the risk-free rate;
- the squared risk-free rate;
- the yield-curve slope;
- the VIX;
- firm leverage.

The final model is estimated using a Two-Way Fixed Effects specification:

    CDS spread change = leverage change + firm fixed effects + date fixed effects + residual

Date fixed effects absorb variables that are common to all firms on a given day, including the observed macroeconomic regressors. Standard errors are clustered by both firm and date to account for serial correlation, heteroskedasticity and cross-sectional dependence.

Principal Component Analysis is subsequently applied to the regression residuals to identify latent patterns in CDS spread changes.

---

### 2. Event Study Around Leverage Shocks

Large positive leverage changes are used as proxy credit events.

For each firm, an event is identified when its leverage change exceeds the firm's own **99th percentile**. Candidate events must be separated by at least 60 available firm observations to reduce overlapping windows.

The event-study window is divided into:

- estimation window: observations from -100 to -61;
- pre-event window: observations from -60 to -1;
- event date: observation 0;
- post-event window: observations from +1 to +5.

Abnormal CDS changes and cumulative abnormal CDS changes are computed relative to the estimation window.

An event is classified as economically relevant when at least one of the following conditions holds:

- maximum daily CDS increase of at least 10 basis points;
- maximum cumulative abnormal CDS increase of at least 20 basis points.

The timing of CDS and equity movements is compared across the full sample, the acute COVID-19 period and the remaining observations.

---

### 3. Bank CDS Factor and Random Matrix Theory

The analysis is repeated on the G-SIB subsample.

PCA is applied to bank-level regression residuals, and the statistical significance of the resulting components is assessed using:

- analytical Marchenko–Pastur bounds;
- the Biroli fat-tail correction;
- Monte Carlo bounds based on rotational shuffling.

Rotational shuffling preserves each bank's time-series dependence while destroying contemporaneous cross-sectional co-movement. It therefore provides a more conservative benchmark for distinguishing common factors from noise.

A rolling PCA is also performed using:

- 130-trading-day estimation windows;
- 22-trading-day steps.

This allows the factor structure and bank loadings to be tracked over time.

---

### 4. Bank–Sovereign Contagion

A Europe-specific banking factor is extracted from the residuals of the ten European G-SIBs.

A VAR model is then estimated using:

- the European banking PC1;
- changes in Spanish sovereign CDS spreads;
- changes in Portuguese sovereign CDS spreads.

The lag order is selected using the Bayesian Information Criterion.

The analysis includes:

- VAR stability tests;
- Granger causality tests;
- orthogonalized impulse response functions;
- residual autocorrelation and ARCH diagnostics;
- GARCH(1,1) models for the residual volatility.

---

## Main Results

### Panel Regression

The estimated coefficient on daily leverage changes is statistically significant but economically modest at the daily frequency.

A one-standard-deviation increase in daily leverage is associated with an increase of approximately **0.6 basis points** in the CDS spread.

The model's within R-squared is approximately **1%**, confirming that observable firm-level variation explains only a small part of daily CDS spread changes.

The first principal component of the residuals explains **24.5%** of residual variance, while the first ten components jointly explain **44.6%**.

---

### Informational Content of CDS Spreads

The event-identification procedure detects:

| Quantity | Result |
|---|---:|
| Leverage shocks | 1,911 |
| Flagged events | 767 |
| Flagged share | 40.1% |
| Firms involved | 235 |
| Bank events | 165 |

For the full flagged sample, the maximum daily CDS increase occurs:

| Timing | Share |
|---|---:|
| Before the leverage shock | 54.4% |
| On the event date | 26.6% |
| After the leverage shock | 19.0% |

The results differ substantially across market conditions.

Outside the acute COVID-19 period, **63.1%** of CDS peaks occur before the leverage shock. This is consistent with CDS spreads incorporating credit information before deterioration becomes visible in the leverage measure.

During the acute COVID-19 period, only **13.3%** of CDS peaks occur before the event, while **57.0%** occur afterwards. Under systemic stress, CDS repricing therefore appears more contemporaneous and persistent.

These results are descriptive and should not be interpreted as proof of informed trading or as a causal amplification mechanism.

---

### Bank Factor Structure

The Random Matrix Theory analysis identifies:

- 4 significant components under the analytical Marchenko–Pastur bound;
- 4 significant components after the Biroli fat-tail correction;
- 3 significant components under the more conservative Monte Carlo bound.

The first principal component does **not** represent a uniform global systemic-risk factor.

Instead, its loadings reveal a clear geographical structure:

- major US investment banks load strongly in one direction;
- European G-SIBs load in the opposite direction;
- Japanese banks and some remaining institutions have loadings close to zero.

The dominant factor is therefore better interpreted as a measure of **relative credit stress between US and European banking systems** rather than as a single global systemic-risk index.

This geographical separation remains visible in the rolling-window analysis and is robust to alternative specifications based on equity returns and one-year CDS contracts.

---

### Sovereign Contagion

The Bayesian Information Criterion selects a stable VAR(1).

The null hypothesis that the European banking factor does not Granger-cause Spanish and Portuguese sovereign CDS changes is rejected jointly:

| Test | p-value |
|---|---:|
| European banking PC1 does not predict Spain and Portugal jointly | 0.030 |
| PC1 coefficient in the Portuguese CDS equation | 0.049 |
| PC1 coefficient in the Spanish CDS equation | 0.715 |
| Spanish CDS changes do not predict Portuguese CDS changes | 0.048 |

The predictive relationship is therefore mainly driven by Portugal.

Impulse response functions show that a shock to the European banking factor is followed by a short-lived increase in both Spanish and Portuguese CDS changes, with a larger response for Portugal.

These findings are consistent with a regional bank–sovereign nexus. However, Granger causality measures predictive content and does not establish a structural causal relationship.

---

## Key Interpretation

The project produces three main conclusions:

1. **CDS spreads display partial early-warning properties in comparatively normal periods.**

2. **During systemic stress, CDS repricing becomes more synchronous, reactive and persistent.**

3. **The dominant global banking factor captures geographical divergence rather than uniform global systemic risk, while a Europe-specific banking factor contains predictive information for sovereign CDS markets.**

---

## Repository Structure

A recommended repository structure is:

    .
    ├── code/
    │   ├── data_preparation/
    │   ├── panel_regression/
    │   ├── event_study/
    │   ├── pca_rmt/
    │   └── var_garch/
    ├── data/
    │   ├── raw/
    │   └── processed/
    ├── figures/
    ├── tables/
    ├── report/
    │   └── final_report.pdf
    └── README.md

The `data/raw` directory should not contain proprietary datasets when the repository is made public.

---

## Reproducing the Analysis

The empirical analysis was implemented in **R**.

To reproduce the results:

1. Obtain the required CDS, leverage, equity and macro-financial datasets.
2. Place the local input files in the appropriate data directory.
3. Update the input and output paths defined in the analysis scripts.
4. Run the data-cleaning and panel-construction scripts.
5. Estimate the panel models and extract the residuals.
6. Run the leverage-shock event study.
7. Execute the PCA and Random Matrix Theory analysis.
8. Estimate the European bank–sovereign VAR and the GARCH robustness models.
9. Generate the figures and tables used in the final report.

Exact reproducibility may depend on access to the original licensed datasets.

---

## Limitations

The main limitations of the analysis are:

- leverage shocks are endogenous and are not exogenous credit announcements;
- leverage depends partly on market capitalization and may mechanically react to equity-price movements;
- the event sample is selected conditional on a sufficiently large CDS response;
- the pre-event window is longer than the post-event window;
- the G-SIB sample remains relatively small;
- PCA factors are statistical objects and require careful economic interpretation;
- Granger causality does not imply structural causality;
- residual volatility remains highly persistent and fat-tailed.

---

## Authors

This project was developed for the Econometrics course at Politecnico di Milano by:

- Arianna Gatta
- Nassim Karimi
- Samuele Lupano
- Marco Gretter

Academic year: **2025–2026**

---

## Main References

- Acharya, V. V., and Johnson, T. C. (2007). *Insider Trading in Credit Derivatives*. Journal of Financial Economics.
- Acharya, V., Drechsler, I., and Schnabl, P. (2014). *A Pyrrhic Victory? Bank Bailouts and Sovereign Credit Risk*. Journal of Finance.
- Barbieri, C., Guerini, M., and Napoletano, M. (2024). *The Anatomy of Government Bond Yields Synchronization in the Eurozone*. Macroeconomic Dynamics.
- Collin-Dufresne, P., Goldstein, R. S., and Martin, J. S. (2001). *The Determinants of Credit Spread Changes*. Journal of Finance.
- Giglio, S. (2016). *Credit Default Swap Spreads and Systemic Financial Risk*. ESRB Working Paper Series.

---

## Disclaimer

This repository contains an academic research project developed for educational purposes. The results do not constitute financial advice, investment recommendations or a production-ready credit-risk model.
