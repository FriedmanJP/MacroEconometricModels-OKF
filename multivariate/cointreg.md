---
type: Feature
title: Cointegrating Regression (FMOLS / CCR / DOLS)
description: Single-equation estimation of a cointegrating vector by fully-modified OLS, canonical cointegrating regression, and dynamic OLS, plus group-mean and pooled panel extensions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/cointreg
tags:
  - cointegration
  - fmols
  - dols
  - ccr
  - multivariate
  - panel
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:16:20Z
sources:
  - id: cointreg-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/cointreg.md
    title: Cointegrating Regression docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-cointreg
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/cointreg
    title: src/cointreg module source
---

# Summary

The `cointreg` module estimates a single cointegrating vector for `y_t` on `I(1)` regressors `x_t` with three asymptotically efficient, endogeneity-corrected estimators: FMOLS (Phillips-Hansen 1990) via an endogeneity-purged regressand plus a one-sided long-run-covariance bias correction, CCR (Park 1992) via a canonical data transformation followed by plain OLS, and DOLS (Saikkonen 1991; Stock-Watson 1993) via leads and lags of `Δx_t` with automatic AIC/BIC selection.
All three build on the shared `lrcov`/`lrcov_oneside` HAC toolkit and return a `CointRegModel` exposing the stacked `(u, Δx)` long-run covariance pieces (`Omega`, `Lambda`, `Sigma`, `omega_uv`) for downstream tests.
`estimate_xtcointreg` aggregates per-unit fits into group-mean (Pedroni 2001) or pooled (Pedroni 2000 FMOLS / Kao-Chiang 2000 DOLS) panel cointegrating regressions.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_cointreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/cointreg/fmols.jl) | `estimate_cointreg(y, X; method=:fmols, trend=:const, kernel=:bartlett, bandwidth=:andrews, leads=:auto, lags=:auto, ic=:aic, dols_se=:lrv)` | Single-equation FMOLS/CCR/DOLS estimation; returns `CointRegModel` |
| [estimate_xtcointreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/cointreg/panel.jl) | `estimate_xtcointreg(pd::PanelData, y, xs...; method=:fmols, pooling=:group, trend=:const, kernel=:bartlett, bandwidth=:andrews, leads=:auto, lags=:auto, ic=:aic, dols_se=:lrv)` | Panel FMOLS/DOLS over a `PanelData`; returns `PanelCointRegModel` |
| [estimate_xtcointreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/cointreg/panel.jl) | `estimate_xtcointreg(y::AbstractVector, X, id, time; kwargs..., xnames=nothing)` | Long-format panel method with unit/time id vectors |
| [CointRegModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/cointreg/types.jl) | `struct CointRegModel{T} <: StatsAPI.RegressionModel` | Single-equation fit: long-run coef, vcov, residuals, HAC bandwidth, `Omega`/`Lambda`/`Sigma`/`omega_uv` |
| [PanelCointRegModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/cointreg/types.jl) | `struct PanelCointRegModel{T} <: StatsAPI.RegressionModel` | Panel fit: pooled or group-mean coef, per-unit fits, Pedroni between-dimension `t` |

# Examples

```julia
using MacroEconometricModels
m = estimate_cointreg(y, x; method=:fmols, trend=:const)
report(m)
md = estimate_cointreg(y, x; method=:dols, leads=:auto, lags=:auto, ic=:aic)
round.(confint(m; level=0.95), digits=4)
```

# See also

* [Vector Error Correction Models](/multivariate/vecm.md) - full-system cointegration with multiple cointegrating vectors
* [Vector Autoregression](/multivariate/var.md) - stationary systems and the `to_var` conversion target
* [Local Projections](/multivariate/lp.md) - impulse responses without cointegration structure
