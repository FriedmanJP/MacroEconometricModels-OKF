---
type: Feature
title: Vector Autoregression (VAR)
description: OLS estimation, lag selection, forecasting, and conditional forecasting for reduced-form VAR(p) models.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/var
tags:
  - var
  - multivariate
  - time-series
  - forecasting
  - identification
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:11:32Z
sources:
  - id: var-manual
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/manual.md
    title: VAR manual page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-var
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/var
    title: src/var module source
---

# Summary

The `var` module implements the reduced-form VAR(p) of Sims (1980): OLS estimation equation-by-equation, information-criteria lag selection, stability checking via the companion matrix, multi-step forecasting with bootstrap or analytic intervals, and Waggoner-Zha conditional forecasts.
`estimate_var` returns a `VARModel` holding the coefficient matrix `B = [c, A_1, ..., A_p]'`, residuals, and the ML residual covariance `Sigma`.
Structural analysis (`irf`, `fevd`, `historical_decomposition`) consumes `VARModel` through the core innovation-accounting layer, and `to_var` converts VECM fits into this same type.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `estimate_var(Y::AbstractMatrix, p::Int; check_stability=true, varnames=nothing)` | OLS estimation of reduced-form VAR(p); returns `VARModel` |
| [estimate_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `estimate_var(df::DataFrame, p::Int; vars=Symbol[], check_stability=true)` | DataFrame method selecting columns via `vars` |
| [select_lag_order](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `select_lag_order(Y::AbstractMatrix, max_p::Int; criterion=:bic)` | Lag choice via `:aic`, `:bic`, or `:hqic` on a common sample |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `forecast(model::VARModel, h::Int; ci_method=:bootstrap, reps=500, conf_level=0.95, stationary_only=false, rng)` | Multi-step point forecast with `:none`, `:bootstrap`, or `:analytic` intervals |
| [conditional_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `conditional_forecast(model::VARModel, conditions, h::Int; Q=nothing, reps=1000, conf_level=0.95, seed=nothing, rng)` | Waggoner-Zha conditional forecast under hard/soft path constraints |
| [conditional_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `conditional_forecast(post::BVARPosterior, conditions, h::Int; ...)` | Conditional forecast integrating over the BVAR posterior |
| [forecast_condition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `forecast_condition(variable, horizon, value; sd=0.0)` | Build one path constraint (`sd=0` hard, `sd>0` soft) |
| [effective_nobs](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/types.jl) | `effective_nobs(model::VARModel)` | Effective sample size `T - p` |
| [vcov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `StatsAPI.vcov(model::VARModel)` | Dof-corrected coefficient covariance |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `StatsAPI.predict(model::VARModel, steps::Int)` | Iterated multi-step point forecast path |
| [r2](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `StatsAPI.r2(model::VARModel)` | Per-equation R-squared |
| [loglikelihood](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `StatsAPI.loglikelihood(model::VARModel)` | Gaussian log-likelihood |
| [confint](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/estimation.jl) | `StatsAPI.confint(model::VARModel; level=0.95)` | Coefficient confidence intervals |
| [is_stationary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/stationarity.jl) | `is_stationary(model::VARModel)` | Stability verdict from companion eigenvalues |
| [companion_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/utils.jl) | `companion_matrix(B::AbstractMatrix, n::Int, p::Int)` | `np x np` companion form of the VAR |
| [VARModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/types.jl) | `struct VARModel{T} <: AbstractVARModel` | Fitted VAR: `Y`, `p`, `B`, `U`, `Sigma`, criteria, `varnames` |
| [VARForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/types.jl) | `struct VARForecast{T} <: AbstractForecastResult{T}` | Forecast path with interval bounds |
| [ForecastCondition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `struct ForecastCondition` | Single variable/horizon/value constraint |
| [ConditionalForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `struct ConditionalForecast{T}` | Constrained forecast path result |

# Examples

```julia
using MacroEconometricModels
model = estimate_var(Y, 4; varnames=["INDPRO", "CPI", "FFR"])
report(model)
p = select_lag_order(Y, 4; criterion=:bic)
result = irf(model, 20; method=:cholesky, ci_type=:bootstrap, reps=50)
report(result)
```

# See also

* [Bayesian VAR](bvar.md) - shrinkage estimation of the same VAR dynamics
* [Vector Error Correction Models](vecm.md) - cointegrated I(1) systems
* [Local Projections](lp.md) - horizon-by-horizon alternative to VAR-based IRFs
* [Factor Models](factor.md) - large-panel dimension reduction feeding VARs
* [Factor-Augmented VAR](/multivariate/favar.md) - VARs augmented with latent factors
