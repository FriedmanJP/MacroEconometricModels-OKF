---
type: Feature
title: Vector Error Correction Models (VECM)
description: Johansen and Engle-Granger estimation, rank selection, restriction testing, and forecasting for cointegrated systems.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/vecm
tags:
  - vecm
  - cointegration
  - multivariate
  - time-series
  - forecasting
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:11:32Z
sources:
  - id: vecm-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/vecm.md
    title: Vector Error Correction Models docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-vecm
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/vecm
    title: src/vecm module source
---

# Summary

The `vecm` module estimates Vector Error Correction Models for cointegrated I(1) systems, separating long-run equilibrium (`alpha * beta'`) from short-run dynamics (`Gamma`).
`estimate_vecm` defaults to Johansen (1991) reduced-rank MLE with automatic trace-test rank selection, and offers Engle-Granger two-step for bivariate rank-1 systems.
`to_var` converts the fit to a VAR in levels so all structural identification schemes apply; `irf`, `fevd`, and `historical_decomposition` accept `VECMModel` directly via that conversion.
Restriction tests on `alpha`/`beta` (including weak exogeneity), SVEC permanent/transitory identification, bootstrap forecasting, and short/long/strong Granger causality complete the toolkit.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_vecm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/estimation.jl) | `estimate_vecm(Y, p; rank=:auto, deterministic=:constant, method=:johansen, significance=0.05, varnames)` | Johansen MLE or Engle-Granger VECM estimation; returns `VECMModel` |
| [select_vecm_rank](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/estimation.jl) | `select_vecm_rank(Y, p; criterion=:trace, significance=0.05, deterministic=:constant)` | Cointegrating rank via `:trace` or `:max_eigen` test |
| [to_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/estimation.jl) | `to_var(vecm::VECMModel)` | Convert VECM to VAR in levels for structural analysis |
| [cointegrating_rank](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `cointegrating_rank(m::VECMModel)` | Fitted cointegrating rank `r` |
| [effective_nobs](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `effective_nobs(m::VECMModel)` | Effective sample size from residual rows |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/forecast.jl) | `forecast(vecm::VECMModel, h::Int; ci_method=:none, reps=500, conf_level=0.95, rng)` | Level iteration preserving cointegration; `:bootstrap`/`:simulation` CIs |
| [granger_causality_vecm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/granger.jl) | `granger_causality_vecm(vecm::VECMModel, cause::Int, effect::Int)` | Short-run, long-run, and strong Wald causality tests |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `irf(vecm::VECMModel, horizon::Int; method=:cholesky, kwargs...)` | Impulse responses via automatic `to_var` conversion |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `fevd(vecm::VECMModel, horizon::Int; method=:cholesky, kwargs...)` | Variance decomposition via automatic `to_var` conversion |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `historical_decomposition(vecm::VECMModel, horizon=effective_nobs(vecm); ...)` | Historical decomposition via automatic `to_var` conversion |
| [test_beta_restriction](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `test_beta_restriction(m::VECMModel, H::AbstractMatrix)` | LR test of `beta = H*phi` (same restriction, every vector) |
| [test_alpha_restriction](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `test_alpha_restriction(m::VECMModel, A::AbstractMatrix)` | LR test of `alpha = A*psi` |
| [test_weak_exogeneity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `test_weak_exogeneity(m::VECMModel, vars)` | LR test of zero `alpha` rows for named variables |
| [test_known_beta](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `test_known_beta(m::VECMModel, b::AbstractMatrix)` | LR test of fully specified `beta = b` |
| [test_joint_restriction](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/analysis.jl) | `test_joint_restriction(m::VECMModel, H::AbstractMatrix, A::AbstractMatrix; ...)` | Joint LR test on `alpha` and `beta` via switching algorithm |
| [identify_svec](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/svec.jl) | `identify_svec(vecm::VECMModel; long_run_zeros=nothing, short_run_zeros=nothing, pattern=nothing, n_starts=5, max_iter=400, rng)` | King-Plosser-Stock-Watson permanent/transitory SVEC identification |
| [permanent_transitory](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/svec.jl) | `permanent_transitory(vecm::VECMModel; method=:gonzalo_ng)` | Gonzalo-Ng or KPSW permanent/transitory decomposition |
| [dof](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/estimation.jl) | `StatsAPI.dof(m::VECMModel)` | Model degrees of freedom |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/estimation.jl) | `StatsAPI.predict(m::VECMModel)` | In-sample fitted values in differences (not levels) |
| [VECMModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `struct VECMModel{T}` | Fitted VECM: `alpha`, `beta`, `Pi`, `Gamma`, `Sigma`, criteria |
| [VECMForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `struct VECMForecast{T}` | Level/difference forecasts with CI bounds |
| [VECMGrangerResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `struct VECMGrangerResult{T}` | Short-run, long-run, and strong Wald statistics |
| [VECMRestrictionTest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/vecm/types.jl) | `struct VECMRestrictionTest{T}` | LR statistic plus re-estimated `restricted_model` |

# Examples

```julia
using MacroEconometricModels
vecm = estimate_vecm(Y, 2; rank=1)
report(vecm)
irfs = irf(vecm, 20; method=:cholesky)
fc = forecast(vecm, 10; ci_method=:bootstrap, reps=50, conf_level=0.95)
g = granger_causality_vecm(vecm, 1, 2)
report(g)
```

# See also

* [Vector Autoregression](var.md) - stationary systems and the `to_var` target type
* [Bayesian VAR](bvar.md) - shrinkage estimation of VAR dynamics
* [Local Projections](lp.md) - direct impulse-response estimation without cointegration structure
* [Factor Models](factor.md) - large-panel dimension reduction
