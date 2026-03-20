# VIX Time Series Analysis

A time series analysis of the CBOE Volatility Index (VIX) using SARIMA and GARCH methodologies.

**Author:** Aarti Garaye  
**Course:** PSTAT 174 - Time Series Analysis  
**Institution:** UC Santa Barbara  
**Date:** March 2026

## Overview

This project analyzes the CBOE Volatility Index (VIX), also known as the market's "fear gauge," using classical econometric methods. The analysis combines Box-Jenkins SARIMA modeling for conditional mean dynamics with GARCH models to capture time-varying volatility.

## Key Findings

- ARIMA(1,1,1) best captures the mean dynamics of VIX
- GARCH(1,1) with Student-t errors effectively models volatility clustering
- VIX exhibits high volatility persistence with shocks lasting approximately 7 days
- Long memory analysis reveals fractional integration around d = 0.38
- Heavy tails in the distribution require Student-t errors rather than normal distribution

## Installation

Required R packages:

```r
install.packages(c("readr", "forecast", "astsa", "tseries", 
                   "rugarch", "ggplot2", "knitr", "kableExtra", 
                   "moments", "fracdiff"))
```

## Usage

### Running the Analysis

Open and knit the main analysis file:

```r
rmarkdown::render("VIXTimeSeriesAnalysis.Rmd")
```

### Generating the Presentation

Render the Quarto presentation:

```r
quarto::quarto_render("Presentation/VIX_Presentation.qmd")
```

## Methods

### SARIMA Analysis

Applied Box-Jenkins methodology to identify and estimate ARIMA(1,1,1) model. Conducted stationarity tests, ACF/PACF analysis, and diagnostic checking of residuals.

### GARCH Modeling

Fitted GARCH(1,1) model with Student-t distributed errors to capture volatility clustering and heavy tails observed in VIX returns.

### Long Memory Investigation

Explored fractional integration using Haslett-Raftery maximum likelihood estimation to explain the slow ACF decay pattern.

## Data Source

VIX data obtained from Federal Reserve Economic Data (FRED):
https://fred.stlouisfed.org/series/VIXCLS

Daily observations from January 1990 to February 2026.

## Results

Final models selected:
- Mean equation: ARIMA(1,1,1)
- Variance equation: GARCH(1,1) with Student-t errors
- Volatility persistence: High (alpha + beta = 0.91)
- Half-life of shocks: 7.4 days

## Presentation

A small 25 slide deck was used to present the high level idea of this project. It is live on the github pages for this repository. [This](https://aarti-garaye.github.io/VIXTimeSeriesAnalysis/) is the link to the slides.

## Files

- **VIX_Final_Project.Rmd**: Main analysis report
- **VIX_Presentation.qmd**: Presentation slides
- **VIXCLS.csv**: Raw VIX data
- **references.bib**: Bibliography
- **ucsb-theme.scss**: Presentation styling

## References

See references.bib for full citations.

Key references:
- Bollerslev (1986) - GARCH methodology
- Engle (1982) - ARCH theory
- Hansen & Lunde (2005) - GARCH model comparison

## Contact

Aarti Garaye  
aartigaraye@ucsb.edu

## License

MIT License
