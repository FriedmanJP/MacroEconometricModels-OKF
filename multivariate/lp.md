---
type: Feature
title: Local Projections (LP)
description: Horizon-by-horizon impulse-response estimation with IV, smooth, state-dependent, and propensity-score variants plus LP-FEVD and forecasting.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/lp
tags:
  - local-projections
  - impulse-responses
  - multivariate
  - time-series
  - identification
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:11:32Z
sources:
  - id: lp-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/lp.md
    title: Local Projections docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-lp
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/lp
    title: src/lp module source
---

# Summary

The `lp` module estimates impulse responses by Jordà (2005) Local Projections: a separate predictive regression at each horizon with Newey-West HAC standard errors (automatic bandwidth covering the MA(h-1) overlap).
Four variants extend the core estimator: LP-IV with external instruments (Stock & Watson 2018) including weak-instrument-robust inference, Smooth LP with penalized B-splines (Barnichon & Brownlees 2019), regime-varying State LP (Auerbach & Gorodnichenko 2012), and propensity-score LP for discrete treatments (Angrist, Jordà & Kuersteiner 2018).
`structural_lp` pairs VAR-based identification with LP response estimation, `lp_fevd` gives the R²-based variance decomposition (Gorodnichenko & Lee 2019), and `forecast` produces direct multi-step forecasts.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/core.jl) | `estimate_lp(Y, shock_var::Int, horizon::Int; lags=4, response_vars, cov_type=:newey_west, bandwidth=0, conf_level=0.95, varnames)` | Horizon-by-horizon OLS LP; returns `LPModel` |
| [estimate_lp_multi](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/core.jl) | `estimate_lp_multi(Y, shock_vars::Vector{Int}, horizon::Int; kwargs...)` | One `LPModel` per shock variable |
| [estimate_lp_cholesky](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/core.jl) | `estimate_lp_cholesky(Y, horizon::Int; lags=4, cov_type=:newey_west, kwargs...)` | Recursively identified LP per structural shock |
| [structural_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/core.jl) | `structural_lp(Y, horizon::Int; method=:cholesky, lags=4, var_lags=nothing, cov_type=:newey_west, conf_level=0.95, ci_type=:none, reps=200, ...)` | VAR identification with LP response estimation |
| [compare_var_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/core.jl) | `compare_var_lp(Y, horizon::Int; lags=4)` | Side-by-side VAR vs LP impulse responses |
| [lp_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `lp_irf(model::LPModel; conf_level=0.95, ...)` | IRF with analytical or fixed-design bootstrap bands |
| [cumulative_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `cumulative_irf(irf::LPImpulseResponse)` | Cumulated LP impulse responses |
| [estimate_lp_iv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/iv.jl) | `estimate_lp_iv(Y, shock_var::Int, instruments, horizon::Int; lags=4, response_vars, cov_type=:newey_west, bandwidth=0, varnames)` | 2SLS LP-IV with external instruments |
| [lp_iv_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/iv.jl) | `lp_iv_irf(model::LPIVModel; conf_level=0.95)` | Instrumented IRF with confidence intervals |
| [weak_instrument_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/iv.jl) | `weak_instrument_test(model::LPIVModel; threshold=10.0)` | HAC first-stage F across horizons |
| [sargan_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/iv.jl) | `sargan_test(model::LPIVModel, h::Int)` | Overidentification J test at horizon `h` |
| [montiel_olea_pflueger_f](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/weak_iv.jl) | `montiel_olea_pflueger_f(model::LPIVModel; tau=0.10, ...)` | Heteroskedasticity-robust effective F with bias critical values |
| [lp_iv_ar_band](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/weak_iv.jl) | `lp_iv_ar_band(model::LPIVModel; level=0.95, n_grid=401, ...)` | Anderson-Rubin bands valid at any instrument strength |
| [estimate_smooth_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/smooth.jl) | `estimate_smooth_lp(Y, shock_var::Int, horizon::Int; degree=3, n_knots=4, lambda=0.0, lags=4, response_vars, cov_type=:newey_west, bandwidth=0, varnames)` | Penalized B-spline smooth LP |
| [smooth_lp_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/smooth.jl) | `smooth_lp_irf(model::SmoothLPModel; conf_level=0.95)` | Smoothed IRF propagating cross-horizon covariance |
| [bspline_basis](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/smooth.jl) | `bspline_basis(horizons, degree::Int, n_interior_knots::Int; ...)` | Cubic B-spline basis over horizons |
| [cross_validate_lambda](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/smooth.jl) | `cross_validate_lambda(Y, shock_var::Int, horizon::Int; lambda_grid, k_folds=5, kwargs...)` | K-fold choice of the smoothing penalty |
| [compare_smooth_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/smooth.jl) | `compare_smooth_lp(Y, shock_var::Int, horizon::Int; lambda=1.0, kwargs...)` | Standard vs smooth IRF with variance-reduction ratio |
| [estimate_state_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/state.jl) | `estimate_state_lp(Y, shock_var::Int, state_var, horizon::Int; gamma=:estimate, threshold=:estimate, lags=4, response_vars, cov_type=:newey_west, bandwidth=0, varnames)` | Logistic smooth-transition state-dependent LP |
| [state_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/state.jl) | `state_irf(model::StateLPModel; regime=:both, conf_level=0.95)` | Regime-specific impulse responses |
| [estimate_transition_params](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/state.jl) | `estimate_transition_params(state_var, Y, shock_var::Int; method=:nlls, gamma_init=1.5, lags=4, c_init=:median)` | Estimate smooth-transition `(gamma, c)` |
| [test_regime_difference](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/state.jl) | `test_regime_difference(model::StateLPModel; h=nothing)` | Test for regime-varying responses |
| [estimate_propensity_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `estimate_propensity_lp(Y, treatment::AbstractVector{Bool}, covariates, horizon::Int; ps_method=:logit, trimming=(0.01, 0.99), lags=4, response_vars, cov_type=:newey_west, bandwidth=0, varnames)` | Inverse-propensity-weighted LP for discrete treatments |
| [doubly_robust_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `doubly_robust_lp(Y, treatment::AbstractVector{Bool}, covariates, horizon::Int; ps_method=:logit, trimming=(0.01, 0.99), lags=4, response_vars, cov_type=:newey_west, bandwidth=0, varnames)` | Doubly robust LP combining weighting and outcome regression |
| [propensity_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `propensity_irf(model::PropensityLPModel; conf_level=0.95)` | Treatment-effect IRF from propensity LP |
| [estimate_propensity_score](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `estimate_propensity_score(treatment::AbstractVector{Bool}, X; ...)` | Propensity score via logit/probit |
| [inverse_propensity_weights](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `inverse_propensity_weights(treatment::AbstractVector{Bool}, propensity; ...)` | Trimmed IPW weights |
| [propensity_diagnostics](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/propensity.jl) | `propensity_diagnostics(model::PropensityLPModel)` | Overlap and balance diagnostics |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/forecast.jl) | `forecast(lp::LPModel, shock_path::AbstractVector; ci_method=:analytical, conf_level=0.95, n_boot=500, rng)` | Direct multi-step forecast along a shock path |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/forecast.jl) | `forecast(slp::StructuralLP, shock_idx::Int, shock_path::AbstractVector; ci_method=:analytical, conf_level=0.95, n_boot=500, rng)` | Structural-shock conditional forecast |
| [lp_fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/fevd.jl) | `lp_fevd(slp::StructuralLP, horizon::Int; method=:r2, bias_correct=true, n_boot=500, conf_level=0.95, var_lags=nothing, seed=nothing, rng)` | R²-based LP forecast error variance decomposition |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/fevd.jl) | `fevd(slp::StructuralLP, horizon::Int; kwargs...)` | Alias of `lp_fevd` |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/hd.jl) | `historical_decomposition(slp::StructuralLP, T_hd::Int; shock_names=nothing)` | Historical decomposition from structural-LP IRFs |
| [LPForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types.jl) | `struct LPForecast{T} <: AbstractForecastResult{T}` | Direct-forecast values, SEs, confidence bands |
| [LPFEVD](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types.jl) | `struct LPFEVD{T} <: AbstractFEVD` | FEVD proportions, bias-corrected shares, bands |
| [LPModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types.jl) | `struct LPModel{T} <: AbstractLPModel` | Per-horizon coefficients, residuals, vcov, effective samples |
| [LPImpulseResponse](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types.jl) | `struct LPImpulseResponse{T} <: AbstractLPImpulseResponse` | LP IRF values, standard errors, confidence bands |
| [StructuralLP](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types.jl) | `struct StructuralLP{T} <: AbstractFrequentistResult` | Identified LP system over all structural shocks |
| [LPIVModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types_variants.jl) | `struct LPIVModel{T} <: AbstractLPModel` | LP-IV fit with first-stage F statistics |
| [SmoothLPModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types_variants.jl) | `struct SmoothLPModel{T} <: AbstractLPModel` | Penalized-spline LP fit |
| [StateLPModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types_variants.jl) | `struct StateLPModel{T} <: AbstractLPModel` | Regime-varying LP fit with transition weights |
| [PropensityLPModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/lp/types_variants.jl) | `struct PropensityLPModel{T} <: AbstractLPModel` | Weighted LP fit for discrete treatments |

# Examples

```julia
using MacroEconometricModels
lp = estimate_lp(Y, 3, 20; lags=4, cov_type=:newey_west, varnames=vnames)
result = lp_irf(lp; conf_level=0.95)
report(result)
slp = structural_lp(Y, 20; method=:cholesky, lags=4, varnames=vnames)
report(slp)
```

# See also

* [Vector Autoregression](var.md) - system-based impulse responses and the `compare_var_lp` baseline
* [Bayesian VAR](bvar.md) - Bayesian impulse responses with credible bands
* [Vector Error Correction Models](vecm.md) - impulse responses for cointegrated systems
* [Factor Models](factor.md) - factor-augmented settings where LP responses apply
* [Policy Counterfactuals](../policy-counterfactuals/counterfactual.md) - structural-LP menus feeding sufficient-statistics policy analysis
