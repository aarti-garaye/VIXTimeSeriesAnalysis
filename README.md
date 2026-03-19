# Modeling Volatility in Financial Markets: VIX Time Series Analysis

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![R](https://img.shields.io/badge/R-4.0+-blue.svg)](https://www.r-project.org/)
[![Quarto](https://img.shields.io/badge/Quarto-1.3+-orange.svg)](https://quarto.org/)

A comprehensive time series analysis of the CBOE Volatility Index (VIX) using SARIMA and GARCH methodologies. This project examines volatility dynamics, long memory properties, and forecasting performance of the market's "fear gauge."

**Course**: PSTAT 174 - Time Series Analysis  
**Institution**: UC Santa Barbara  
**Author**: Aarti Garaye  
**Date**: March 2026

---

## 📊 Project Overview

This analysis applies classical econometric methods to model and forecast VIX dynamics:

- **Box-Jenkins SARIMA**: Captures conditional mean dynamics
- **GARCH Models**: Models time-varying volatility and clustering
- **Long Memory Analysis**: Investigates fractional integration (d ≈ 0.38)
- **Student-t Distribution**: Accounts for heavy tails in returns

### Key Findings

- ✅ **ARIMA(1,1,1)** best captures mean dynamics
- ✅ **GARCH(1,1)** with Student-t errors models volatility clustering
- ✅ **High persistence**: α + β = 0.91 (volatility shocks last ~7 days)
- ✅ **Heavy tails**: Student-t d.o.f. ν = 4.72 (extreme events common)
- ✅ **Long memory**: Fractional d ≈ 0.38 confirms slow ACF decay

---

## 📁 Repository Structure

```
vix-time-series-analysis/
├── Data/
│   └── VIXCLS.csv                          # VIX daily data (1990-2026)
├── Code/
│   ├── VIX_Final_Project.Rmd               # Main analysis (R Markdown)
│   ├── fracdiff_section.R                  # Long memory analysis
│   └── fracdiff_rmarkdown_section.Rmd      # Fracdiff for report
├── Presentation/
│   ├── VIX_Presentation.qmd                # Quarto slides
│   ├── ucsb-theme.scss                     # UCSB styling
│   └── UC_Santa_Barbara_Wordmark_Navy_RGB.png
├── Output/
│   ├── VIX_Final_Project.pdf               # Final report
│   └── VIX_Presentation.html               # Presentation slides
├── references.bib                          # Bibliography
├── FRACDIFF_INTEGRATION_GUIDE.R           # Integration instructions
├── PRESENTATION_SETUP_GUIDE.R             # Presentation setup
└── README.md                               # This file
```

---

## 🚀 Getting Started

### Prerequisites

**Required Software:**
- R (≥ 4.0.0)
- RStudio (recommended)
- Quarto (for presentation)

**Required R Packages:**
```r
install.packages(c(
  "readr",        # Data import
  "forecast",     # ARIMA modeling
  "astsa",        # Time series analysis
  "tseries",      # Stationarity tests
  "rugarch",      # GARCH models
  "fracdiff",     # Long memory (optional)
  "ggplot2",      # Visualization
  "knitr",        # Report generation
  "kableExtra",   # Table formatting
  "moments"       # Distribution statistics
))
```

### Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/vix-time-series-analysis.git
   cd vix-time-series-analysis
   ```

2. **Open in RStudio**
   ```r
   # Open the main analysis file
   file.edit("Code/VIX_Final_Project.Rmd")
   ```

3. **Knit the report**
   ```r
   rmarkdown::render("Code/VIX_Final_Project.Rmd")
   ```

4. **View the presentation**
   ```r
   quarto::quarto_render("Presentation/VIX_Presentation.qmd")
   ```

---

## 📈 Methodology

### 1. Box-Jenkins SARIMA

**Process:**
1. **Identification**: ACF/PACF analysis, ADF stationarity tests
2. **Estimation**: Maximum likelihood parameter estimation
3. **Diagnostics**: Residual analysis, Ljung-Box tests
4. **Forecasting**: 12-step ahead predictions

**Selected Model**: ARIMA(1,1,1)
```
(1 - φB)(1 - B)log(VIX_t) = (1 + θB)w_t
```
Where: φ = 0.821, θ = -0.905

### 2. GARCH Modeling

**Specification**: GARCH(1,1) with Student-t errors

**Mean equation:**
```
r_t = μ + φ₁r_{t-1} + θ₁ε_{t-1} + ε_t
```

**Variance equation:**
```
σ²_t = ω + α₁ε²_{t-1} + β₁σ²_{t-1}
```

**Key Parameters:**
- α₁ = 0.134 (ARCH effect)
- β₁ = 0.776 (GARCH persistence)
- ν = 4.72 (Student-t d.o.f.)

### 3. Fractional Integration (Optional)

Estimated using Haslett-Raftery MLE:
- **d̂ = 0.38** (stationary long memory)
- Explains slow ACF decay (400+ lags)
- ARFIMA theoretically superior, ARIMA practical

---

## 📊 Results Summary

| Model | AIC | Key Finding |
|-------|-----|-------------|
| ARIMA(0,1,1) | -23240.51 | MA(1) baseline |
| ARIMA(1,1,0) | -23232.92 | AR(1) baseline |
| **ARIMA(1,1,1)** | **-23364.86** | **Best fit** |
| GARCH(1,1) | Log-Lik: 12806.70 | Captures volatility clustering |

### Volatility Persistence

| Measure | Value | Interpretation |
|---------|-------|----------------|
| α₁ + β₁ | 0.910 | Very high persistence |
| Half-life | 7.4 days | Shocks decay slowly |
| Unconditional σ² | 0.00463 | Long-run variance |

### Diagnostic Performance

✅ **ARIMA Residuals**: Mostly white noise (some heavy tails)  
✅ **GARCH Residuals**: Captures heteroskedasticity  
✅ **Forecasts**: Mean reversion with expanding uncertainty  
✅ **Heavy Tails**: Student-t distribution essential

---

## 📝 Key Insights

### 1. **Long Memory Discovery**
VIX exhibits fractional integration (d ≈ 0.38), not standard I(0) or I(1):
- Shocks persist longer than exponential decay
- ACF remains significant for 400+ lags
- Explains why both d=0 and d=1 appear stationary

### 2. **Volatility Regimes Matter**
β₁ (0.776) >> α₁ (0.134) indicates:
- Past volatility more important than recent shocks
- Volatility regimes persist (crisis periods last weeks)
- Mean reversion is slow but present

### 3. **Extreme Tail Risk**
Student-t ν = 4.72 confirms:
- VIX has much fatter tails than normal distribution
- Extreme events (crises) more frequent than normal models predict
- Standard risk models severely underestimate tail risk

### 4. **Practical Applications**
- **VaR Calculations**: GARCH volatility forecasts improve risk estimates
- **Options Pricing**: Time-varying volatility crucial for derivatives
- **Trading**: Mean reversion suggests contrarian strategies
- **Portfolio Management**: VIX forecasts inform dynamic allocation

---

## 🔬 Future Research Directions

- **Regime-Switching Models**: Explicitly model crisis vs. normal periods
- **Asymmetric GARCH**: GJR-GARCH for leverage effects
- **Multivariate Analysis**: Joint VIX-S&P 500 dynamics with DCC-GARCH
- **High-Frequency Data**: Realized volatility with HAR models
- **Machine Learning**: LSTM networks for nonlinear patterns
- **ARFIMA-GARCH**: Combine long memory in both mean and variance

---

## 📚 References

### Key Papers
- Bollerslev, T. (1986). "Generalized Autoregressive Conditional Heteroskedasticity." *Journal of Econometrics*, 31(3), 307-327.
- Engle, R. F. (1982). "Autoregressive Conditional Heteroscedasticity." *Econometrica*, 50(4), 987-1007.
- Hansen, P. R., & Lunde, A. (2005). "A Forecast Comparison of Volatility Models: Does Anything Beat a GARCH(1,1)?" *Journal of Applied Econometrics*, 20(7), 873-889.

### Data Source
- Federal Reserve Economic Data (FRED)
- Series: VIXCLS (CBOE Volatility Index)
- URL: https://fred.stlouisfed.org/series/VIXCLS

Full bibliography available in `references.bib`

---

## 📧 Contact

**Aarti Garaye**  
Email: aartigaraye@ucsb.edu  
Institution: UC Santa Barbara, Department of Statistics

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Professor Tomoyuki Ichiba** - Course instruction and guidance
- **UCSB PSTAT Department** - Support and resources
- **FRED Database** - High-quality VIX data
- **R Community** - Excellent packages and documentation

---

## 📖 How to Cite

If you use this work, please cite:

```bibtex
@misc{garaye2026vix,
  author = {Garaye, Aarti},
  title = {Modeling Volatility in Financial Markets: A Time Series Analysis of the CBOE Volatility Index},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub Repository},
  howpublished = {\url{https://github.com/yourusername/vix-time-series-analysis}}
}
```

---

## 🔍 Additional Resources

- **Full Report**: [`Output/VIX_Final_Project.pdf`](Output/VIX_Final_Project.pdf)
- **Presentation Slides**: [`Output/VIX_Presentation.html`](Output/VIX_Presentation.html)
- **Fracdiff Guide**: [`FRACDIFF_INTEGRATION_GUIDE.R`](FRACDIFF_INTEGRATION_GUIDE.R)
- **Setup Instructions**: [`PRESENTATION_SETUP_GUIDE.R`](PRESENTATION_SETUP_GUIDE.R)

---

**⭐ If you found this analysis helpful, please consider starring this repository!**
