---
type: Feature
title: Shared Kernel (Core Utilities, Innovation Accounting, Identification)
description: Innovation accounting, SVAR identification, HAC covariance, Kalman kernel, reproducibility, and display utilities shared by every estimator.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/core
tags:
  - infrastructure
  - core
  - irf
  - fevd
  - identification
  - hac
  - kalman
  - reproducibility
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/infrastructure
  at: 2026-09-25T01:38:10Z
sources:
  - id: data-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/data.md
    title: Data Management docs (reproducibility, persistence, logging, tables)
  - id: src-core
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/core
    title: src/core module source
---

# Summary

The `core` module is the package's shared kernel: the innovation-accounting layer (`irf`, `fevd`, `historical_decomposition`) consumed by every VAR-family fit, the unified SVAR identification dispatcher (`compute_Q`) with proxy, sign, narrative, and long-run schemes, HAC/robust covariance estimators, a consolidated Kalman kernel, reproducibility manifests with bit-for-bit `reproduce`, JLD2 model persistence (`save_model`/`load_model`), Tables.jl/CSV exports, logging, and display backends. The table below covers the primary entry points; set-identification internals (`arias.jl`, `uhlig.jl`, `maxshare.jl`, `robust_bayes.jl`, `ab.jl`) and bootstrap-scheme helpers are intentionally truncated.

# Functions

| Function | Signature | Role |
|---|---|---|
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `irf(model::VARModel, horizon::Int; method=:cholesky, ci_type=:none, reps=200, conf_level=0.95, ...)` | Frequentist IRFs with bootstrap, theoretical, or identified-set bands |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `irf(post::BVARPosterior, horizon::Int; method=:cholesky, quantiles=[0.16, 0.5, 0.84], ...)` | Bayesian IRFs with posterior quantiles |
| [lp_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `lp_irf(model::LPModel; conf_level=0.95, ci_type=:analytical, ...)` | LP IRFs with analytical or fixed-design bootstrap bands |
| [ma_coefficients](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `ma_coefficients(B, n, p, H)` | Reduced-form MA coefficients `Φ_0 = I, …, Φ_{H-1}` |
| [cumulative_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/irf.jl) | `cumulative_irf(irf_result::ImpulseResponse)` | Cumulative IRFs, cumulating draws before quantiles |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/fevd.jl) | `fevd(model::VARModel, horizon::Int; method=:cholesky, ...)` | Forecast error variance decomposition shares |
| [generalized_fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/fevd.jl) | `generalized_fevd(model::VARModel, horizon::Int; normalize=false, ...)` | Ordering-invariant Pesaran-Shin generalized FEVD |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/hd.jl) | `historical_decomposition(model::VARModel, horizon=effective_nobs(model); method=:cholesky, ...)` | Shock contributions plus initial conditions |
| [contribution](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/hd.jl) | `contribution(hd::HistoricalDecomposition, var::Int, shock::Int)` | One variable-by-shock contribution time series |
| [verify_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/hd.jl) | `verify_decomposition(hd::HistoricalDecomposition; tol=1e-10)` | Check contributions plus initial conditions equal actual |
| [identify_proxy](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `identify_proxy(model::VARModel, Z::AbstractMatrix; shocks=..., normalize=:unit_effect, ...)` | External-instrument (proxy) SVAR identification |
| [identify_sign](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `identify_sign(model::VARModel, horizon::Int, check_func::Function; max_draws=1000, store_all=false, ...)` | Sign-restriction rotations via Haar draws |
| [identify_narrative](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `identify_narrative(model::VARModel, horizon::Int, sign_check::Function, narrative_check::Function; ...)` | Combined sign and narrative restrictions |
| [identify_long_run](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `identify_long_run(model::VARModel)` | Blanchard-Quah long-run rotation |
| [compute_Q](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `compute_Q(model::VARModel, method::Symbol; horizon=1, ...)` | Unified identification dispatcher returning `Q` |
| [compute_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `compute_irf(model::VARModel, Q::AbstractMatrix, horizon::Int)` | `horizon × n × n` IRF array for a given rotation |
| [compute_structural_shocks](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `compute_structural_shocks(model::VARModel, Q::AbstractMatrix)` | Recover structural shocks from residuals |
| [generate_Q](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `generate_Q(n::Int; rng, seed)` | Haar-uniform random orthogonal matrix |
| [ProxySVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `struct ProxySVARResult{T} <: AbstractAnalysisResult` | Proxy SVAR result with first-stage F and reliability |
| [irf_percentiles](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `irf_percentiles(s::SignIdentifiedSet; quantiles=[0.16, 0.5, 0.84])` | Pointwise IRF quantiles over an identified set |
| [median_target](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `median_target(s)` | Fry-Pagan median-target rotation of an identified set |
| [joint_band](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `joint_band(s; level=0.68, loss=:absolute)` | Inoue-Kilian joint credible band |
| [sup_t_band](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `sup_t_band(s; level=0.68)` | Montiel Olea-Plagborg-Møller sup-t simultaneous band |
| [label_shocks](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `label_shocks(result::AbstractNonGaussianSVAR; by=:restrictions, ...)` | Relabel statistical-ID shocks by signed permutation |
| [newey_west](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `newey_west(X, residuals; bandwidth=0, kernel=:bartlett, prewhiten=false, ...)` | Newey-West HAC covariance matrix |
| [white_vcov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `white_vcov(X, residuals; variant=:hc0, ...)` | White heteroskedasticity-robust covariance (HC0-HC3) |
| [driscoll_kraay](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `driscoll_kraay(X, u; bandwidth=0, kernel=:bartlett, ...)` | Driscoll-Kraay covariance |
| [robust_vcov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `robust_vcov(X, residuals, estimator::Union{NeweyWestEstimator,WhiteEstimator,DriscollKraayEstimator})` | Dispatch on the concrete estimator object (vector or matrix residuals) |
| [optimal_bandwidth_nw](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `optimal_bandwidth_nw(residuals; kernel=:bartlett)` | Andrews (1991) plug-in HAC bandwidth |
| [long_run_covariance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `long_run_covariance(X; bandwidth=0, kernel=:bartlett)` | Kernel long-run covariance matrix |
| [register_cov_estimator!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/covariance.jl) | `register_cov_estimator!(name::Symbol, ::Type{E})` | Register a custom covariance estimator type |
| [lrvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/lrvar.jl) | `lrvar(U; kernel=:bartlett, bandwidth=:andrews, prewhiten=false, demean=true)` | Two-sided kernel long-run variance/covariance |
| [lrcov_oneside](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/lrvar.jl) | `lrcov_oneside(U; kernel=:bartlett, bandwidth=:andrews, ...)` | One-sided long-run covariance for FMOLS/CCR |
| [varhac](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/lrvar.jl) | `varhac(U; ic=:aic, max_lag=:auto, demean=true)` | Parametric VARHAC long-run covariance |
| [construct_var_matrices](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/utils.jl) | `construct_var_matrices(Y, p)` | VAR design matrices `Y_eff = X * B + U` |
| [companion_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/utils.jl) | `companion_matrix(B, n, p)` | VAR(p) to VAR(1) companion form |
| [robust_inv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/utils.jl) | `robust_inv(A; silent=false, rcond_tol=...)` | Matrix inverse with pseudo-inverse fallback |
| [safe_cholesky](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/utils.jl) | `safe_cholesky(A; jitter=1e-10, silent=false)` | Cholesky factor with scale-relative jitter |
| [capture_manifest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/repro.jl) | `capture_manifest(; seed=nothing, settings=...)` | Capture a reproducibility manifest |
| [reproduce](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/repro.jl) | `reproduce(result)` | Re-run a randomized result and compare bit-for-bit |
| [save_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/serial/api.jl) | `save_model(model, path; note="", compress=false)` | Persist a model, container, or bundle to JLD2 |
| [load_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/serial/api.jl) | `load_model(path)` | Reconstruct a saved model or bundle dict |
| [model_info](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/serial/bundles.jl) | `model_info(path)` | Read a model file header without reconstructing |
| [set_log_level](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/logging.jl) | `set_log_level(level)` | Set the global minimum log level |
| [with_min_level](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/logging.jl) | `with_min_level(f, level)` | Run `f()` at a scoped minimum log level |
| [set_display_backend](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/display.jl) | `set_display_backend(backend::Symbol)` | Set the PrettyTables backend (`:text`, `:latex`, `:html`) |
| [with_display_backend](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/display.jl) | `with_display_backend(f, backend::Symbol)` | Run `f()` under a scoped display backend |
| [long_table](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/tables.jl) | `long_table(result)` | Tidy long table of an array-valued result |
| [write_csv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/tables.jl) | `write_csv(result, path)` | Export a result or table to CSV |
| [KalmanFilterStore](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/kalman_kernel.jl) | `KalmanFilterStore{T}(n_state, T_obs; innovations=false)` | Storage sink for the consolidated Kalman filter |
| [MacroModelError](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/exceptions.jl) | `abstract type MacroModelError <: Exception` | Root of the typed error hierarchy |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
d = fix(apply_tcode(fred[:, ["INDPRO", "CPIAUCSL", "FEDFUNDS"]]))
model = estimate_var(d, 2)
irfs = irf(model, 20; method=:cholesky)
hd = historical_decomposition(model)
verify_decomposition(hd)
```

# See also

* [Data Management](/infrastructure/data.md) - typed containers that feed every core routine
* [Simulation (DGPs)](/infrastructure/dgp.md) - population IRF/FEVD/HD moments validating this layer
* [Visualization](/infrastructure/plotting.md) - plots of IRF, FEVD, and HD results
* [Vector Autoregression](/multivariate/var.md) - the canonical consumer of core innovation accounting
* [Bayesian VAR](/multivariate/bvar.md) - posterior draws behind the Bayesian methods
* [Hypothesis Tests](/testing/teststat.md) - tests built on the HAC/long-run-variance toolkit
