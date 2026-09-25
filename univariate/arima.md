---
type: Feature
title: ARIMA Models
description: Univariate AR, MA, ARMA, ARIMA, SARIMA, and ARFIMA estimation with order selection and forecasting.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/arima
tags: [arima, sarima, arfima, univariate, order-selection, forecasting]
status: approved
reviewed_by: chung9207
reviewed_at: 2026-09-25
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-arima
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/arima.md
    title: Upstream ARIMA Models documentation
  - id: upstream-src-arima
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/arima
    title: Upstream arima source directory
---

# Summary

The `arima` module implements the full Box-Jenkins workflow for univariate
ARIMA-class models: AR(p) via OLS or exact MLE, MA(q) and ARMA(p,q) via CSS,
exact MLE, or CSS-MLE, ARIMA(p,d,q) via d-fold differencing, multiplicative
seasonal SARIMA(p,d,q)(P,D,Q)s, and fractional ARFIMA(p,d,q) for long memory.
Exact MLE runs through a Kalman-filter state-space likelihood; order selection
is by AIC/BIC grid search (`select_arima_order`), stepwise or exhaustive
`auto_arima`, and `auto_sarima` with HEGY/KPSS differencing selection.
All models implement the StatsAPI regression interface and `report()`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_ar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/estimation.jl) | `estimate_ar(y, p; method=:ols, include_intercept=true)` | Estimate AR(p) by OLS or exact MLE; returns ARModel |
| [estimate_ma](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/estimation.jl) | `estimate_ma(y, q; method=:css_mle, include_intercept=true, max_iter=500)` | Estimate MA(q); returns MAModel |
| [estimate_arma](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/estimation.jl) | `estimate_arma(y, p, q; method=:css_mle, include_intercept=true, max_iter=500)` | Estimate ARMA(p,q); returns ARMAModel |
| [estimate_arima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/estimation.jl) | `estimate_arima(y, p, d, q; method=:css_mle, include_intercept=true, max_iter=500)` | Difference d times then fit ARMA(p,q); returns ARIMAModel |
| [estimate_sarima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/sarima.jl) | `estimate_sarima(y, p, d, q, P, D, Q, s; method=:css_mle, include_intercept=true, max_iter=500)` | Multiplicative seasonal ARIMA via expanded ARMA; returns SARIMAModel |
| [estimate_arfima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/arfima.jl) | `estimate_arfima(y, p, q; method=:css, d0=nothing, trunc=200, max_iter=500)` | Fractional ARFIMA with d in (-0.5, 0.5); returns ARFIMAModel |
| [select_arima_order](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/selection.jl) | `select_arima_order(y, max_p, max_q; criterion=:bic, d=0, method=:css_mle, include_intercept=true)` | AIC/BIC grid search over (p,q); returns ARIMAOrderSelection |
| [auto_arima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/selection.jl) | `auto_arima(y; max_p=5, max_q=5, max_d=2, criterion=:bic, method=:css_mle, include_intercept=true, stepwise=true)` | ADF-based d selection plus stepwise (p,q) search |
| [auto_sarima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/sarima.jl) | `auto_sarima(y, s; d=nothing, D=nothing, max_p=2, max_q=2, max_P=1, max_Q=1, criterion=:aic, method=:css_mle, include_intercept=true)` | Seasonal order search with HEGY/KPSS differencing selection |
| [ic_table](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/selection.jl) | `ic_table(result; criterion=:bic)` | Render the AIC/BIC grid from a selection result |
| [gph_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/arfima.jl) | `gph_test(y; m=:default, trim=0)` | Geweke-Porter-Hudak log-periodogram estimate of d; returns GPHResult |
| [local_whittle](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/arfima.jl) | `local_whittle(y; m=:default)` | Robinson Gaussian semiparametric estimate of d; returns LocalWhittleResult |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/forecast.jl) | `forecast(model, h; conf_level=0.95)` | Multi-step forecasts with psi-weight intervals; returns ARIMAForecast |
| [StatsAPI interface](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arima/types.jl) | `fit`, `predict`, `coef`, `nobs`, `dof`, `residuals`, `fitted`, `loglikelihood`, `aic`, `bic`, `r2`, `stderror`, `vcov`, `confint` | Standard regression-model accessors for every ARIMA-class model |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
y = filter(isfinite, diff(log.(fred[:, "CPIAUCSL"])))[end-99:end]
ar = estimate_ar(y, 2)
fc = forecast(ar, 12; conf_level=0.95)
report(fc)
```

# See also

* [Spectral Analysis](/univariate/spectral.md) - ACF/PACF correlograms for order identification and Ljung-Box diagnostics.
* [Time Series Filters](/univariate/filters.md) - Beveridge-Nelson decomposition selects its ARMA order via `auto_arima`.
* [X-13ARIMA-SEATS](/univariate/x13.md) - Seasonal adjustment built on seasonal ARIMA models.
* [MacroEconometricModels.jl overview](/overview.md) - Package feature map.
