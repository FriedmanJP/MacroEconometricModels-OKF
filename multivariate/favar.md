---
type: Feature
title: Factor-Augmented VAR (FAVAR)
description: Two-step and Bayesian factor-augmented VARs that compress a large panel into latent factors, estimate a VAR on factors plus key observables, and map structural results back to the full panel.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/favar
tags:
  - favar
  - factor-models
  - multivariate
  - time-series
  - identification
  - forecasting
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:16:20Z
sources:
  - id: favar-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/favar.md
    title: Factor-Augmented VAR docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-favar
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/favar
    title: src/favar module source
---

# Summary

The `favar` module implements the Factor-Augmented VAR of Bernanke, Boivin & Eliasz (2005): extract `r` latent factors from a large panel by principal components, remove double-counting against the key observed variables (e.g. the policy rate), and estimate a VAR on the augmented `[F, Y_key]` system.
The two-step estimator (`method=:two_step`) treats the PCA factors as data; the Bayesian estimator (`method=:bayesian`) jointly Gibbs-samples factors, loadings, and VAR parameters with Carter-Kohn forward-filtering backward-sampling, so credible bands include factor-extraction uncertainty.
Structural analysis (`irf`, `fevd`, `historical_decomposition`) delegates to the VAR or BVAR infrastructure through `to_var`, and `favar_panel_irf` / `favar_panel_forecast` map results back to every panel series through the loadings `Lambda` plus the direct channel `Lambda_y`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_favar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/estimation.jl) | `estimate_favar(X, Y_key::AbstractMatrix, r::Int, p::Int; method=:two_step, panel_varnames, n_draws=5000, burnin=1000, seed, rng)` | Two-step or Bayesian FAVAR with key variables as a matrix; returns `FAVARModel` or `BayesianFAVAR` |
| [estimate_favar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/estimation.jl) | `estimate_favar(X, key_indices::Vector{Int}, r::Int, p::Int; ...)` | Same, with key variables as panel column indices |
| [to_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/types.jl) | `to_var(favar::FAVARModel)` | Convert to `VARModel` for structural-analysis dispatch |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `irf(favar::FAVARModel, horizon::Int; kwargs...)` | Augmented-system IRFs via `to_var` (all VAR identification methods) |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `fevd(favar::FAVARModel, horizon::Int; kwargs...)` | Augmented-system FEVD via `to_var` |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `historical_decomposition(favar::FAVARModel, horizon=effective_nobs(favar); kwargs...)` | Augmented-system historical decomposition via `to_var` |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `irf(bfavar::BayesianFAVAR, horizon::Int; kwargs...)` | Draw-by-draw Bayesian IRFs via `BVARPosterior` delegation |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `fevd(bfavar::BayesianFAVAR, horizon::Int; kwargs...)` | Bayesian FEVD via `BVARPosterior` delegation |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `historical_decomposition(bfavar::BayesianFAVAR, horizon=0; kwargs...)` | Bayesian historical decomposition via `BVARPosterior` delegation |
| [favar_panel_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `favar_panel_irf(favar::FAVARModel, irf_result::ImpulseResponse)` | Map augmented IRFs to all `N` panel series through `Lambda` and `Lambda_y` |
| [favar_panel_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `favar_panel_irf(bfavar::BayesianFAVAR, irf_result::BayesianImpulseResponse)` | Bayesian panel IRFs with quantiles recomputed in panel space |
| [favar_panel_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/analysis.jl) | `favar_panel_forecast(favar::FAVARModel, fc::VARForecast)` | Map augmented forecasts to the full panel |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/forecast.jl) | `forecast(favar::FAVARModel, h::Int; kwargs...)` | Augmented-system forecast delegating to `forecast(::VARModel, h)` |
| [FAVARModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/types.jl) | `struct FAVARModel{T} <: AbstractVARModel` | Two-step fit: augmented VAR plus factors, loadings, `Lambda_y`, panel metadata |
| [BayesianFAVAR](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/favar/types.jl) | `struct BayesianFAVAR{T}` | Bayesian fit: posterior draws of VAR params, factors, loadings, `lambda_y` |

# Examples

```julia
using MacroEconometricModels
favar = estimate_favar(X, [5], 2, 2; panel_varnames=panel_names)
report(favar)
r = irf(favar, 20; method=:cholesky)
r_panel = favar_panel_irf(favar, r)
report(r_panel)
fc = forecast(favar, 6)
fc_panel = favar_panel_forecast(favar, fc)
```

# See also

* [Factor Models](/multivariate/factor.md) - factor extraction, factor-count selection, and structural DFMs
* [Vector Autoregression](/multivariate/var.md) - the augmented-system estimator and identification machinery behind `to_var`
* [Bayesian VAR](/multivariate/bvar.md) - posterior infrastructure the Bayesian FAVAR delegates to
* [Local Projections](/multivariate/lp.md) - direct impulse-response estimation without a factor structure
* [Vector Error Correction Models](/multivariate/vecm.md) - cointegrated systems as an alternative to factor dynamics
