# Time Series Causal Analysis of U.S. Air Pollution (2000–2016)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python) ![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter) ![License](https://img.shields.io/badge/License-Academic-lightgrey) ![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> **Quantifying causal relationships between atmospheric pollutants across U.S. states using Granger causality, OLS regression, and time series econometrics — enabling data-driven environmental policy decisions.**

---

## Table of Contents

- [Business Impact](#business-impact)
- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Technical Architecture](#technical-architecture)
- [Results](#results)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Tech Stack](#tech-stack)
- [Authors](#authors)

---

## Business Impact

Air quality degradation costs the U.S. economy hundreds of billions of dollars annually through healthcare expenditures, lost productivity, and regulatory non-compliance penalties. This project delivers a **statistically rigorous causal inference framework** that directly addresses critical environmental and economic questions:

- **Policy Targeting:** By identifying which pollutants causally drive ozone (O3) concentrations, regulatory agencies (EPA, state DEQs) can prioritize enforcement resources on the highest-impact emission sources — reducing compliance costs and maximizing health outcomes.
- **Predictive Risk Modeling:** The OLS regression model (R² = 0.596) enables forecasting of ozone AQI changes from upstream NO2 and CO measurements, providing a leading indicator for air quality alerts and public health advisories.
- **Regional Strategy:** State-level pollutant trend analysis supports granular resource allocation for environmental compliance programs, infrastructure investment, and industrial permitting decisions.
- **Environmental ESG Reporting:** Organizations operating in affected states can leverage causal pollutant maps to quantify emissions impact and demonstrate regulatory compliance within ESG frameworks.

---

## Project Overview

This project conducts a comprehensive **time series causal analysis** of four major air pollutants — Carbon Monoxide (CO), Nitrogen Dioxide (NO2), Ozone (O3), and Sulfur Dioxide (SO2) — across all U.S. states using EPA monitoring data spanning 2000–2016.

The analytical pipeline progresses from exploratory data analysis through stationarity testing, multicollinearity diagnostics, regression modeling, and Granger causality inference — forming a complete econometric study of inter-pollutant dynamics.

**Core Question:** *Do changes in NO2 and CO concentrations causally precede and predict changes in ground-level ozone — and if so, with what lag structure?*

---

## Key Findings

| Finding | Detail |
|---|---|
| **Granger Causality: NO2 → O3** | NO2 Granger-causes O3 at lags 1 and 2 (p < 0.001) |
| **Granger Causality: CO → O3** | CO Granger-causes O3 at lags 1 and 2 (p < 0.001) |
| **OLS Model Performance** | R² = 0.596 on first-differenced monthly data |
| **High Inter-Pollutant Correlation** | Pearson correlation ~0.9 between pollutant AQI measures |
| **Non-Stationarity Addressed** | First differencing confirmed stationarity (ADF test) |
| **Multicollinearity Detected** | High VIF values; addressed via differencing and variable selection |
| **Significant Predictors** | NO2 and CO first-difference lags are statistically significant drivers of O3_diff |

---

## Dataset

- **Source:** U.S. Environmental Protection Agency (EPA) — National Ambient Air Quality Standards (NAAQS)
- **Temporal Coverage:** January 2000 – December 2016 (monthly aggregates)
- **Geographic Scope:** All U.S. states
- **Pollutants Tracked:**
  - **NO2** — Nitrogen Dioxide (AQI & mean concentration)
  - **O3** — Ozone (AQI & mean concentration)
  - **SO2** — Sulfur Dioxide (AQI & mean concentration)
  - **CO** — Carbon Monoxide (AQI & mean concentration)
- **Key Variables:** AQI index, pollutant mean (ppm/ppb), state, date

---

## Methodology

The project follows a structured econometric workflow:

### 1. Exploratory Data Analysis (EDA)
- Descriptive statistics and null value audit across all pollutant series
- Correlation heatmap revealing strong inter-pollutant relationships (~0.9)
- State-wise NO2 time series visualization to identify regional patterns

### 2. Multicollinearity Diagnostics
- Variance Inflation Factor (VIF) analysis to quantify predictor redundancy
- Informs feature selection for downstream regression modeling

### 3. Stationarity Testing
- **Augmented Dickey-Fuller (ADF) Test** applied to all pollutant series
- First-order differencing applied to achieve stationarity for regression and causality testing

### 4. OLS Regression Modeling
- Ordinary Least Squares regression on first-differenced series
- Target variable: `O3_diff` (monthly change in ozone AQI)
- Predictors: lagged differences of NO2, CO, SO2 concentrations
- Model evaluation: R², coefficient significance (t-statistics, p-values)

### 5. Granger Causality Testing
- Bivariate Granger causality tests at lags 1 and 2
- Tests whether NO2 and CO time series contain predictive information about future O3 values beyond O3's own history
- Statistical threshold: p < 0.05

---

## Technical Architecture

```
Data Ingestion (EPA CSV)
        │
        ▼
Exploratory Data Analysis
   ├── Descriptive Statistics
   ├── Null Value Analysis
   └── Correlation Heatmap
        │
        ▼
Multicollinearity Check (VIF)
        │
        ▼
Stationarity Testing (ADF)
        │
        ▼
First-Order Differencing
        │
        ├──► OLS Regression Model (R²=0.596)
        │
        └──► Granger Causality Tests
                 ├── NO2 → O3 (p<0.001, lags 1-2)
                 └── CO  → O3 (p<0.001, lags 1-2)
```

---

## Results

### Granger Causality Summary

```
Hypothesis: NO2 does NOT Granger-cause O3
  Lag 1: F-statistic significant, p < 0.001  → REJECTED
  Lag 2: F-statistic significant, p < 0.001  → REJECTED

Hypothesis: CO does NOT Granger-cause O3
  Lag 1: F-statistic significant, p < 0.001  → REJECTED
  Lag 2: F-statistic significant, p < 0.001  → REJECTED
```

**Interpretation:** Changes in nitrogen dioxide and carbon monoxide concentrations statistically precede and predict changes in ozone levels, providing strong causal evidence that these primary pollutants drive secondary ozone formation — a finding with direct implications for emission source prioritization.

### OLS Regression Model

```
Dependent Variable:  O3_diff (First-differenced Ozone AQI)
R-squared:           0.596
Significant Predictors:
  - NO2_diff_lag1    (p < 0.05)
  - CO_diff_lag1     (p < 0.05)
  - NO2_diff_lag2    (p < 0.05)
```

---

## Repository Structure

```
Time-Series-Causal-Analysis-Project/
├── Time_Series_Analysis.ipynb   # Full analysis pipeline (EDA → Causality)
├── Group6_Final_ReportNew.pdf   # Academic research report (UTD, 2023)
└── README.md                    # Project documentation
```

---

## Getting Started

### Prerequisites

```bash
pip install numpy pandas matplotlib seaborn statsmodels scikit-learn linearmodels
```

### Running the Analysis

1. Clone the repository:
```bash
git clone https://github.com/ManojMareedu/Time-Series-Causal-Analysis-Project.git
cd Time-Series-Causal-Analysis-Project
```

2. Launch Jupyter Notebook:
```bash
jupyter notebook Time_Series_Analysis.ipynb
```

3. Execute cells sequentially — the notebook is self-contained and produces all EDA plots, model outputs, and causality test results inline.

> **Note:** The notebook was originally developed on Google Colab with TPU acceleration. For local execution, the `!pip install linearmodels` cell handles all additional dependencies.

---

## Tech Stack

| Category | Tools |
|---|---|
| **Language** | Python 3.x |
| **Data Manipulation** | Pandas, NumPy |
| **Statistical Modeling** | Statsmodels (OLS, ADF, Granger), Scikit-learn |
| **Visualization** | Matplotlib, Seaborn |
| **Environment** | Jupyter Notebook, Google Colab (TPU) |
| **Domain** | Time Series Econometrics, Causal Inference, Environmental Analytics |

---

## Authors

Developed as a graduate research project at the **University of Texas at Dallas** (2023).

**Team:** Manoj Mareedu, Prriyamvradha Parthasarathi, Premi Jawahar Vasagam, Mira Radhakrishnan, Sofia Rajan, Martin Navarro, Vyshnavi Gangineni, Siva Renuka Chowdary Nandigam

---

## Future Enhancements

- [ ] Incorporate ARIMA / SARIMA / VAR models for multivariate time series forecasting
- [ ] Extend Granger causality to bidirectional and panel data frameworks
- [ ] Interactive geospatial dashboard (Plotly/Dash) for state-level pollutant monitoring
- [ ] Real-time EPA data integration via API pipeline
- [ ] Deep learning approaches (LSTM, Temporal Fusion Transformer) for AQI prediction

---

*This project demonstrates applied expertise in time series analysis, econometric causal inference, and environmental data science — skill sets directly applicable to data science, ML engineering, and quantitative analytics roles.*
