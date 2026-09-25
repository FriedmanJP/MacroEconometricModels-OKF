---
type: Feature
title: Factor Models
description: Static, dynamic, generalized dynamic, and structural factor models for compressing large macro panels into latent factors.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/factor
tags:
  - factor-models
  - dfm
  - multivariate
  - forecasting
  - dimension-reduction
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:11:32Z
sources:
  - id: factor-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/factormodels.md
    title: Factor Models docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-factor
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/factor
    title: src/factor module source
---

# Summary

The `factor` module compresses panels of hundreds of indicators into a few latent common factors through four estimators: static principal components (Stock & Watson 2002a) with block-restricted EM, dynamic factor models with explicit factor-VAR dynamics estimated two-step or by EM Kalman smoother (Doz, Giannone & Reichlin 2011, 2012), the frequency-domain generalized dynamic factor model (Forni, Hallin, Lippi & Reichlin 2000, 2005), and structural DFMs with Cholesky or observable sign restrictions (Forni et al. 2009).
Model selection spans Bai & Ng (2002) static IC1-IC3, Hallin-Liska, Bai-Ng (2007), and Amengual-Watson dynamic-factor counts, plus AIC/BIC over the DFM `(r, p)` grid.
Forecasts extrapolate the factor VAR and project through loadings, with theoretical, bootstrap, or simulation intervals.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_factors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `estimate_factors(X, r::Int; standardize=true, blocks=nothing, varnames=nothing)` | Static PCA factors with optional block-restricted EM |
| [ic_criteria](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `ic_criteria(X, max_factors::Int; standardize=true)` | Bai & Ng (2002) IC1-IC3 static factor count |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `forecast(m::FactorModel, h::Int; p=1, ci_method=:theoretical, conf_level=0.95, n_boot=1000, rng)` | Factor-VAR forecast from a static fit |
| [residuals](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `StatsAPI.residuals(m::FactorModel)` | Idiosyncratic residuals `X - F*Lambda'` |
| [r2](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `StatsAPI.r2(m::FactorModel)` | Per-variable R-squared from common factors |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `StatsAPI.predict/nobs/dof(m::FactorModel)` | Common component, observation count, PCA degrees of freedom |
| [estimate_dynamic_factors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `estimate_dynamic_factors(X, r::Int, p::Int; method=:twostep, standardize=true, max_iter=100, tol=1e-6, diagonal_idio=true)` | DFM via two-step PCA+VAR or EM Kalman smoother |
| [ic_criteria_dynamic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `ic_criteria_dynamic(X, max_r::Int, max_p::Int; standardize=true, method=:twostep)` | AIC/BIC grid over factor count and VAR lags |
| [companion_matrix_factors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `companion_matrix_factors(m::DynamicFactorModel)` | Companion form of the factor VAR |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `forecast(m::DynamicFactorModel, h::Int; ci_method=:theoretical, conf_level=0.95, n_boot=1000, ci=false, rng)` | DFM forecast with theoretical/bootstrap/simulation intervals |
| [residuals](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `StatsAPI.residuals(m::DynamicFactorModel)` | Idiosyncratic residuals of the DFM |
| [r2](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `StatsAPI.r2(m::DynamicFactorModel)` | Per-variable R-squared of the DFM |
| [dof](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `StatsAPI.dof(m::DynamicFactorModel)` | DFM degrees of freedom |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `StatsAPI.predict/nobs/loglikelihood/aic/bic(m::DynamicFactorModel)` | Common component, obs count, Kalman loglik and ICs |
| [estimate_gdfm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `estimate_gdfm(X, q::Int; standardize=true, bandwidth=0, kernel=:bartlett, r=0, varnames=nothing, spectral=:lag_window)` | Frequency-domain GDFM with lag-window or smoothed-periodogram spectrum |
| [hallin_liska](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `hallin_liska(X, q_max::Int; c_grid, subpanels=4, bandwidth=0, kernel=:bartlett, penalty=:p1, spectral=:lag_window, standardize=true)` | Hallin-Liska (2007) dynamic factor count |
| [bai_ng_q](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `bai_ng_q(X, r::Int; p=1, δ=0.1, m=1, standardize=true)` | Bai-Ng (2007) dynamic factor count from `r` static factors |
| [amengual_watson_q](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `amengual_watson_q(X, r::Int, p::Int=1; standardize=true)` | Amengual-Watson (2007) dynamic factor count |
| [ic_criteria_gdfm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `ic_criteria_gdfm(X, max_q::Int; standardize=true, bandwidth=0, kernel=:bartlett, spectral=:lag_window)` | Heuristic eigenvalue-ratio and variance-share `q` choice |
| [common_variance_share](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `common_variance_share(m::GeneralizedDynamicFactorModel)` | Per-series share of variance in the common component |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `forecast(model::GeneralizedDynamicFactorModel, h::Int; method=:ar, ...)` | GDFM forecast via per-factor AR(1) extrapolation |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `historical_decomposition(gdfm::GeneralizedDynamicFactorModel; ...)` | Common/idiosyncratic historical decomposition |
| [r2](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `StatsAPI.r2(m::GeneralizedDynamicFactorModel)` | Per-variable R-squared of the GDFM |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `StatsAPI.predict/residuals/nobs/dof(m::GeneralizedDynamicFactorModel)` | Common component, idiosyncratic residuals, obs count, dof |
| [estimate_structural_dfm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `estimate_structural_dfm(X, q::Int; identification=:cholesky, p=1, p_max=8, check_stability=true, H=40, r=0, method=:fglr, ...)` | Structural DFM (FGLR 2009) with Cholesky or sign identification |
| [varindex](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `varindex(m::StructuralDFM, name::AbstractString)` | Panel column index by variable name |
| [structural_shocks](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `structural_shocks(sdfm::StructuralDFM)` | Identified common structural shocks |
| [is_stable](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `is_stable(m::StructuralDFM)` | Stability check on the factor VAR |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `irf(sdfm::StructuralDFM, horizon::Int; point=:auto, ci_type=:none, reps=200, conf_level=0.95, ...)` | Structural impulse responses of factors/panel |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `fevd(sdfm::StructuralDFM, horizon::Int; space=:factor, include_idiosyncratic=false, idio_model=:white, kwargs...)` | Variance decomposition in factor or panel space |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `forecast(sdfm::StructuralDFM, h::Int; ci_method=:none, reps=200, conf_level=0.95, rng)` | Panel forecast through SDFM loadings |
| [sdfm_panel_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `sdfm_panel_irf(sdfm::StructuralDFM, H::Int)` | Panel-wide structural impulse responses |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `StatsAPI.predict/residuals/nobs/dof/coef/aic/bic(m::StructuralDFM)` | GDFM-delegated fit accessors plus factor-VAR coef and ICs |
| [FactorModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/static.jl) | `struct FactorModel{T} <: AbstractFactorModel` | Static fit: factors, loadings, eigenvalues, variance shares |
| [DynamicFactorModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/dynamic.jl) | `struct DynamicFactorModel{T}` | DFM fit: factor VAR, covariances, likelihood, convergence |
| [GeneralizedDynamicFactorModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/generalized.jl) | `struct GeneralizedDynamicFactorModel{T}` | GDFM fit: spectral loadings, common/idiosyncratic split |
| [StructuralDFM](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/structural.jl) | `struct StructuralDFM{T}` | Identified DFM: impact matrix, factor VAR, panel loadings |
| [FactorForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/factor/kalman.jl) | `struct FactorForecast{T}` | Factor and observable forecasts with bands |

# Examples

```julia
using MacroEconometricModels
fm = estimate_factors(X, 3; standardize=true)
report(fm)
ic = ic_criteria(X, 10)
(ic.r_IC1, ic.r_IC2, ic.r_IC3)
dfm = estimate_dynamic_factors(X, 3, 1; method=:twostep, standardize=true)
fc = forecast(dfm, 12; ci_method=:bootstrap, n_boot=50)
report(fc)
```

# See also

* [Vector Autoregression](var.md) - small-system dynamics behind factor-augmented VARs
* [Bayesian VAR](bvar.md) - shrinkage alternative for medium-scale systems
* [Vector Error Correction Models](vecm.md) - cointegration structure factors do not impose
* [Local Projections](lp.md) - direct responses usable alongside factor compression
* [Factor-Augmented VAR](favar.md) - VARs on observed plus latent factors
