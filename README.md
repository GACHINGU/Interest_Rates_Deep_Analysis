# Interest_Rates_Deep_Analysis

# 🇰🇪 Kenya Monetary Policy — Data Science & Structural Analysis

> **Lead Research Scientist:** Stephen Munene | Policy Analysis Unit
> **Simulated educational project — not an official CBK publication**

---

##  Project Overview

This project presents a full data science and econometric analysis of Kenya's interest rates and inflation dynamics over a 65-year period (1960–2024), using real historical data from the Central Bank of Kenya and the World Bank.

It was built in three progressive layers:

| Layer | Output | Description |
|-------|--------|-------------|
| 1 | Descriptive Notebook | 53-year statistical analysis of both series |
| 2 | Structural Modelling Notebook | Deep econometric models (VECM, IRF, FEVD, GARCH, Regime-Switching) |
| 3 | Non_Tech_Presentations AS White Papers (PDF) | Policy documents addressed to the CBK Governor, MPC, National Treasury, and Wananchi (common Kenyan People) |

---

##  Repository Structure

Interest_Rates_Deep_Analysis/
│
├── data/
│   ├── kenya_interest_rates.csv        # Annual interest rates 1971–2023
    ├── Kenya_Interest_Data_Pipeline_Script # How I got the Data
│   └── kenya_inflation_data.csv        # Annual CPI inflation 1960–2024
│
├── notebooks/
│   ├── 01_descriptive_analysis.ipynb   # Time series, distributions, correlations
│   └── 02_structural_modelling.ipynb   # VECM, IRF, FEVD, GARCH, Markov regime-switching
│
├── Non_Tech_Presentations/
│   ├── CBK_MPC_WhitePaper_2024.pdf               # First white paper (descriptive analysis)
│   └── CBK_Deep_Modelling_WhitePaper.pdf  # Deep structural modelling white paper
│
├── Visuals/                            # All generated charts (PNG)
│
└── README.md

---

## Notebook 1 — Descriptive Analysis

**File:** `notebooks/01_descriptive_analysis.ipynb`

### What it covers:
- Descriptive statistics (mean, std dev, skewness, kurtosis) for both series
- Time series plots with annotated historical events
- Distribution analysis with Q-Q plots and Shapiro-Wilk normality tests
- Rolling 10-year volatility analysis
- Decade-by-decade regime breakdown
- ADF stationarity tests
- Pearson and Spearman correlation analysis
- Cross-correlation at lags ±10 years
- Rolling correlation (structural break detection)
- Granger causality tests (both directions, lags 1–4)
- Engle-Granger cointegration test
- Summary dashboard (12 charts in one figure)

### Key findings:
- Interest rate mean: **6.23%** | Std dev: **7.18 pp**
- Inflation mean: **9.70%** | Peak: **45.98% (1993)**
- Pearson r = **-0.318** (p = 0.020) — negative correlation, statistically significant
- Rolling correlation is **structurally unstable** — three distinct regimes identified

---

## Notebook 2 — Deep Structural Modelling

**File:** `notebooks/02_structural_modelling.ipynb`

Every cell is written with plain-English explanations. No prior knowledge of econometrics is required to follow it.

### Models included:

| Model | Purpose | Plain English |
|-------|---------|---------------|
| ADF + KPSS | Stationarity testing | Is each series stable or wandering? |
| Johansen Test | Cointegration | Are the two series leashed together long-run? |
| VECM | Vector Error Correction | Short-run dynamics + long-run equilibrium correction |
| IRF | Impulse Response Functions | What happens to inflation after a CBK rate hike? |
| FEVD | Variance Decomposition | How much of inflation is explained by interest rates? |
| Markov Regime-Switching | Regime identification | When was Kenya in a high-inflation regime? |
| GARCH(1,1) | Volatility modelling | How has inflation uncertainty changed over time? |

### Key findings:
- **Cointegration confirmed** — interest rates and inflation share a long-run equilibrium
- **IRF:** A 1pp rate hike takes **2–5 years** to reduce inflation; year 1 may see a slight increase (price puzzle)
- **FEVD:** Interest rates explain only **~14%** of inflation forecast variance — 86% is supply-driven (food, fuel, FX)
- **GARCH persistence** close to 1.0 — once Kenya enters a volatile inflation period, it tends to stay there
- **Four regimes identified:** Financial Repression (1971–90), Crisis Era (1991–2003), Reform Era (2004–13), Modern Era (2014–23)

---

## White Papers

### White Paper 1 — Descriptive Analysis
Addressed to the CBK Governor and MPC.
Covers the 53-year statistical picture with 12 original charts.
Formal policy language with detailed econometric interpretation.

### White Paper 2 — Deep Structural Modelling
Addressed to **four audiences simultaneously:**
- **H.E. The Governor, Central Bank of Kenya** — long-run relationship, transmission weakness, GARCH risk framework
- **The Monetary Policy Committee** — five actionable recommendations (forward-looking framework, fan charts, regime monitoring)
- **The National Treasury** — fiscal dominance, supply-side investment, exchange rate discipline
- 🇰🇪 **The People of Kenya (Wananchi)** — plain-English explanation using unga prices, the KES 10,000 savings example, and what every Kenyan can do

> The Wananchi section is written entirely in accessible English with Swahili phrases, using everyday Kenyan examples — no finance background required.

---

## Tech Stack

Python 3.12
├── pandas          — data loading and manipulation
├── numpy           — numerical computation
├── matplotlib      — all visualisations
├── seaborn         — statistical plot styling
├── scipy           — correlation, normality tests
├── statsmodels     — ADF, KPSS, Johansen, VECM, Granger, Markov regime-switching
├── arch            — GARCH(1,1) volatility model
├── reportlab       — PDF white paper generation
└── nbformat        — programmatic Jupyter notebook construction

---

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/kenya-monetary-policy.git
cd kenya-monetary-policy
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels arch reportlab nbformat jupyter
```

### 3. Run the descriptive analysis notebook
```bash
jupyter notebook notebooks/01_descriptive_analysis.ipynb
```

### 4. Run the structural modelling notebook
```bash
jupyter notebook notebooks/02_structural_modelling.ipynb
```

### 5. Regenerate the white papers (optional)
```bash
python scripts/generate_whitepaper_1.py
python scripts/generate_whitepaper_2.py
```

---

##  Data Sources

| Dataset | Source | Coverage |
|---------|--------|----------|
| Kenya Interest Rate (lending rate) | Central Bank of Kenya / World Bank WDI | 1971–2023 |
| Kenya CPI Inflation | Kenya National Bureau of Statistics / World Bank WDI | 1960–2024 |

---

##  Methodology Notes

- All models are run on **annual data** — intra-year dynamics are not captured
- The VECM uses the **Johansen cointegration rank** as input — rank is selected by the trace statistic at 5% significance
- FEVD is computed on a **companion VAR in first differences** (since VECMResults does not expose a `.fevd()` method in this version of statsmodels)
- The Markov Regime-Switching model uses **MarkovAutoregression (AR(1))** with `switching_variance=True` and `k_regimes=2`
- GARCH is fitted with a **Student-t error distribution** to account for the fat-tailed, right-skewed nature of Kenya's inflation series (excess kurtosis: +5.89)
- All charts use a Kenya-inspired colour palette: CBK green (`#006400`), deep red (`#8B0000`), and gold (`#C5A028`)

---

##  Design Philosophy

> *Good analysis is only as valuable as its ability to communicate clearly.*

Every notebook cell is written with:
- A plain-English comment block explaining **what** the code does and **why**
- An explanation of **what to look for** in each chart before it is drawn
- A Kenya-specific interpretation of every result after it is produced

The goal: a policy analyst, a journalist, a student, or a curious Kenyan should be able to read this notebook top to bottom and genuinely understand every step.

---

##  Disclaimer

This is a **simulated project created for educational purposes**. It is not an official publication of the Central Bank of Kenya, the Monetary Policy Committee, the National Treasury, or any affiliated institution. All white papers, letters, and policy recommendations are illustrative of the type of analysis that central bank research units produce — they are not binding policy positions.

Data used is publicly available historical data from the Central Bank of Kenya and the World Bank.

---

##  Author

**Stephen Munene**
Policy Analysis Unit
*Kenya Monetary Policy Research Project — 2024*

---

*Asante kwa kusoma. Thank you for reading.*