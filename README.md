# Geopolitical Shocks and Energy Volatility
### A Time Series Analysis of Brent Crude Oil Prices (2000–2024)
**TSAC Individual Project — ENSIA 2025/2026**
**Author: Mehdi BOUZOUL**

---

## Overview

This project applies the classical **Box-Jenkins ARIMA methodology** to weekly Brent crude oil prices, analysing the statistical structure of price changes and producing short-term forecasts. A key finding is the quantification of the **geopolitical risk premium** caused by the 2026 Strait of Hormuz crisis — the gap between what the model predicted under normal market conditions and what actually happened.

The project is directly relevant to Algeria: as an OPEC member whose national budget depends ~60% on hydrocarbon revenues, understanding oil price dynamics is a matter of fiscal policy, not just academic interest.

---

## Project Structure

```
├── TSAC_Oil_Project_Complete.ipynb   # Main Colab notebook (R runtime)
├── data/
│   ├── brent_2000_2024.csv           # Weekly Brent prices, FRED DCOILBRENTEU
│   └── brent_2025_2026.csv           # Actual 2025–early 2026 prices (test set)
└── README.md
```

---

## Methodology

The project follows the full pipeline taught in the TSAC course:

| Step | Method |
|------|--------|
| 1. Exploratory analysis | Time series plot, moving average smoother |
| 2. Variance stabilisation | Log transformation |
| 3. Stationarity testing | ADF test, KPSS test, first differencing |
| 4. Model identification | ACF, PACF of log-returns |
| 5. Model fitting | `sarima()`, `Arima()`, AIC/BIC comparison |
| 6. Diagnostics | Ljung-Box, QQ plot, ARCH test |
| 7. Forecasting | 52-week ahead with prediction intervals |
| 8. Geopolitical shock | War premium = actual − forecast |

---

## Key Results

- **Working series:** Weekly log-returns $r_t = \log P_t - \log P_{t-1}$, confirmed stationary by ADF and KPSS tests
- **Final model:** ARIMA(0, 1, 3) selected by AIC/BIC over ARIMA(0,1,1) and ARIMA(1,1,1)

$$\nabla \log P_t = e_t + 0.2623\, e_{t-1} + 0.0194\, e_{t-2} + 0.1177\, e_{t-3}$$

$$e_t \sim \text{WN}(0,\ 0.002061), \quad \hat{\sigma}_e \approx 4.54\%\ \text{per week}$$

- **Forecast accuracy (2024 evaluation):** MAPE = 1.19% — good accuracy for a univariate commodity model
- **War premium (March 2026):** Actual prices exceeded model forecasts by ~$35/barrel at peak, coinciding with the Hormuz crisis escalation

---

## Data Source

| Dataset | Source | Series | Period |
|---------|--------|--------|--------|
| Brent crude oil price | [FRED — St. Louis Fed](https://fred.stlouisfed.org/series/DCOILBRENTEU) | `DCOILBRENTEU` | 2000–2026 |

Data is stored in `/data` and loaded directly in the notebook via Google Drive — no API calls required to reproduce the analysis.

---

## How to Run

**Option 1 — Google Colab (recommended)**
1. Open `TSAC_Oil_Project_Complete.ipynb` in [Google Colab](https://colab.research.google.com)
2. Set runtime: `Runtime → Change runtime type → R`
3. Run all cells: `Runtime → Run all`

**Option 2 — RStudio local**
1. Install required packages (see first code cell)
2. Replace Drive download links with local CSV paths
3. Run all chunks

### Required R packages
```r
install.packages(c(
  "quantmod", "TSA", "astsa", "forecast",
  "tseries", "FinTS", "lmtest", "ggplot2"
))
```

---

## Main Findings

**Statistical:** Weekly Brent crude log-returns exhibit short-term positive autocorrelation at lags 1 and 3, well-captured by ARIMA(0,1,3). The model performs well under normal market conditions (MAPE = 1.19%) but — as expected — cannot anticipate structural breaks driven by geopolitical events.

**Geopolitical:** The March 2026 Hormuz crisis produced a war premium of ~$35/barrel above the model's prediction at its peak. This is not a model failure — it is a fundamental property of univariate statistical forecasting: past price patterns carry no information about future geopolitical shocks.

**Algerian context:** Algeria's 2026 budget reference price (~$60/barrel) means current elevated prices represent a significant fiscal windfall via Sonatrach revenues. However, this windfall is fragile and entirely contingent on the duration of the crisis.

---

## Known Limitations

1. **Univariate:** The model uses only past prices — no supply, demand, or geopolitical variables
2. **Heteroskedasticity:** ARCH effects confirmed — a GARCH(1,1) extension would better model time-varying volatility
3. **Annual structural breaks:** Four major regime changes (2008, 2014, 2020, 2022) in the training data; standard ARIMA assumes parameter stability across all of them

---

## References

1. Hamilton, J.D. (1983). Oil and the macroeconomy since World War II. *Journal of Political Economy*, 91(2), 228–248.
2. Narayan, P.K., Narayan, S. (2007). Modelling oil price volatility. *Energy Policy*, 35(12), 6549–6553.
3. Cryer, J.D., Chan, K.S. (2008). *Time Series Analysis with Applications in R*. Springer.
4. Shumway, R.H., Stoffer, D.S. (2017). *Time Series Analysis and Its Applications*. Springer.
5. FRED — Federal Reserve Bank of St. Louis. `https://fred.stlouisfed.org/series/DCOILBRENTEU`

---

*TSAC — ENSIA 2024/2025*
