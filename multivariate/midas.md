---
type: Feature
title: MIDAS Regression
description: Mixed-data-sampling regression of a low-frequency target on high-frequency lags through exponential-Almon, Beta, or polynomial weights, with ADL-MIDAS, U-MIDAS, and direct forecasting.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/midas
tags:
  - midas
  - mixed-frequency
  - nowcasting
  - forecasting
  - multivariate
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:16:20Z
sources:
  - id: midas-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/midas.md
    title: MIDAS Regression docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-midas
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/midas
    title: src/midas module source
---

# Summary

The `midas` module regresses a low-frequency target on `K` high-frequency lags of one indicator aggregated through a normalized weight function `w(θ)` (Ghysels, Santa-Clara & Valkanov 2006).
Restricted MIDAS (`:expalmon`, `:beta2`, `:beta3`, `:almon`) is nonlinear least squares with the linear coefficients concentrated out and L-BFGS optimization from a multi-start grid; `p_ar > 0` adds autoregressive lags (ADL-MIDAS, Clements & Galvao 2008); `:umidas` drops the weight function for plain OLS on the stacked lags.
Alignment is positional: the last high-frequency observation anchors to the last target period, and incomplete blocks are dropped.
`forecast` produces a direct point forecast with a Gaussian NLS prediction interval combining residual variance and parameter uncertainty, and `h > 1` estimates the direct multi-step regression.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_midas](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/estimation.jl) | `estimate_midas(y_lf, X_hf; m, K, weights=:expalmon, p_ar=0, poly_degree=2, h=1, max_iter=500)` | Restricted MIDAS NLS, ADL-MIDAS, or U-MIDAS OLS; returns `MidasModel` |
| [midas_weights](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/types.jl) | `midas_weights(m::MidasModel)` | Realized weight curve `w(θ̂)` (length `K`, most-recent-first) |
| [midas_weights](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/types.jl) | `midas_weights(theta, K::Int; kind=:expalmon)` | Evaluate a weight function directly from parameters |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/forecast.jl) | `forecast(m::MidasModel, X_new; y_lags=nothing, level=0.95)` | Direct forecast from a fresh HF block with NLS prediction interval; returns `MidasForecast` |
| [MidasModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/types.jl) | `struct MidasModel{T} <: StatsAPI.RegressionModel` | Fitted MIDAS: `beta`, `theta`, weight curve `w`, Gauss-Newton vcov, fit criteria |
| [MidasForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/midas/types.jl) | `struct MidasForecast{T} <: AbstractForecastResult{T}` | Direct forecast with `se`, interval bounds, horizon, coverage |

# Examples

```julia
using MacroEconometricModels
model = estimate_midas(gdp_q, ip_m; m=3, K=6, weights=:expalmon, p_ar=1)
report(model)
round.(midas_weights(model); digits=4)
fc = forecast(model, ip_new)
report(fc)
```

# See also

* [Factor Models](/multivariate/factor.md) - mixed-frequency DFM as a multi-indicator alternative
* [Bayesian VAR](/multivariate/bvar.md) - mixed-frequency MFVAR for joint multi-series nowcasting
* [Vector Autoregression](/multivariate/var.md) - single-frequency forecasting baseline
