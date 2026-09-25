---
type: Feature
title: Generalized and Simulated Method of Moments (GMM/SMM)
description: One-step, optimal, two-step, and iterated GMM plus simulation-based SMM with Hansen J-tests and Andrews-Lu selection.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/gmm
tags:
  - infrastructure
  - gmm
  - smm
  - moments
  - estimation
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/infrastructure
  at: 2026-09-25T01:38:10Z
sources:
  - id: gmm-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/gmm.md
    title: Generalized and Simulated Method of Moments docs page
  - id: src-gmm
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/gmm
    title: src/gmm module source
---

# Summary

The `gmm` module estimates any model with population moment conditions `E[g(θ)] = 0` by minimizing the quadratic form `Q(θ) = g(θ)' W g(θ)`. `estimate_gmm` offers `:identity`, `:optimal`, `:two_step`, and `:iterated` weighting with HAC long-run covariance for serially correlated moments, an LBFGS optimizer with Nelder-Mead fallback, optional box constraints via `ParameterTransform` (with delta-method standard errors), and a Stock-Yogo first-stage F when `X`/`Z` are supplied. Overidentification is tested with Hansen's J (chi-squared only under efficient weighting) and compared across moment sets with Andrews-Lu MMSC. `estimate_smm` replaces analytic moments with simulated ones (Nelder-Mead first, LBFGS fallback), inflating the covariance by `(1 + 1/τ)`; both result types share the `AbstractGMMModel` StatsAPI interface.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_gmm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `estimate_gmm(moment_fn, theta0, data; weighting=:two_step, max_iter=100, tol=1e-8, hac=true, bandwidth=0, bounds=nothing, X=nothing, Z=nothing, endogenous=nothing)` | GMM estimation under four weighting schemes |
| [GMMModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `struct GMMModel{T} <: AbstractGMMModel` | Fitted GMM: estimates, vcov, J-test, first-stage F |
| [GMMWeighting](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `GMMWeighting(; method=:two_step, max_iter=100, tol=1e-8)` | Weighting specification stored on fits |
| [AbstractGMMModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `abstract type AbstractGMMModel <: StatsAPI.StatisticalModel` | Shared supertype for GMM and SMM results |
| [gmm_objective](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `gmm_objective(theta, moment_fn, data, W)` | Scalar criterion `g(θ)' W g(θ)` |
| [numerical_gradient](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `numerical_gradient(f::Function, x; step=1e-7)` | Central-difference Jacobian of moment means |
| [identity_weighting](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `identity_weighting(n_moments::Int)` | Identity weighting matrix for one-step GMM |
| [optimal_weighting_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `optimal_weighting_matrix(moment_fn, theta, data; hac=true, bandwidth=0)` | Efficient `Ω⁻¹` weighting matrix |
| [minimize_gmm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `minimize_gmm(moment_fn, theta0, data, W; max_iter=100, tol=1e-8)` | LBFGS minimization with Nelder-Mead fallback |
| [j_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `j_test(model::GMMModel)` | Hansen J-test for overidentifying restrictions |
| [gmm_summary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `gmm_summary(model::GMMModel)` | Coefficients, standard errors, and J-test in one NamedTuple |
| [andrews_lu_mmsc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `andrews_lu_mmsc(j_stat, n_instruments::Int, n_params::Int, n_obs::Int; hq_criterion=2.1)` | Andrews-Lu BIC/AIC/HQIC moment selection criteria |
| [linear_gmm_solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `linear_gmm_solve(S_ZX, S_Zy, W)` | Closed-form linear IV-GMM from cross-products |
| [gmm_sandwich_vcov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `gmm_sandwich_vcov(S_ZX, W, D_e)` | Robust one-step sandwich covariance |
| [estimate_lp_gmm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `estimate_lp_gmm(Y, shock_var::Int, horizon::Int; lags=4, weighting=:two_step)` | Local projection via GMM per horizon |
| [lp_gmm_moments](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/gmm.jl) | `lp_gmm_moments(Y, shock_var::Int, h::Int, theta, lags::Int)` | LP moment conditions `E[Z_t ε_{t+h}] = 0` |
| [estimate_smm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `estimate_smm(simulator_fn, moments_fn, theta0, data; sim_ratio=5, burn=100, weighting=:two_step, contributions_fn=nothing, bounds=nothing, hac=true, bandwidth=0, max_iter=1000, tol=1e-8, rng=...)` | Simulated method of moments estimation |
| [SMMModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `struct SMMModel{T} <: AbstractGMMModel` | Fitted SMM, sharing every GMMModel field plus `sim_ratio` |
| [j_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `j_test(m::SMMModel)` | Hansen J-test on an SMM fit |
| [autocovariance_moments](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `autocovariance_moments(data; lags=1, standardize=false)` | Variance-covariance plus diagonal autocovariance vector |
| [autocovariance_moment_contributions](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `autocovariance_moment_contributions(data; lags=1, standardize=false)` | Per-observation contributions matching the moment vector |
| [smm_weighting_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `smm_weighting_matrix(data, contributions_fn; hac=true, bandwidth=0)` | Optimal SMM weighting from contribution HAC |
| [smm_data_covariance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/smm.jl) | `smm_data_covariance(data, contributions_fn; hac=true, bandwidth=0)` | Long-run moment covariance for sandwich standard errors |
| [ParameterTransform](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/transforms.jl) | `ParameterTransform(lower::Vector, upper::Vector)` | Bijective box-constraint transform specification |
| [to_unconstrained](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/transforms.jl) | `to_unconstrained(pt::ParameterTransform, theta)` | Map model space to optimizer space |
| [to_constrained](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/transforms.jl) | `to_constrained(pt::ParameterTransform, phi)` | Map optimizer space to model space |
| [log_jacobian](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/transforms.jl) | `log_jacobian(pt::ParameterTransform, phi)` | Log absolute Jacobian for density correction |
| [transform_jacobian](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/gmm/transforms.jl) | `transform_jacobian(pt::ParameterTransform, phi)` | Diagonal Jacobian for delta-method standard errors |

# Examples

```julia
using MacroEconometricModels, Random
Random.seed!(400)
n = 500
Z = randn(n, 3)
u = randn(n)
X = Z * [0.5, 0.3, 0.2] .+ 0.5 .* u
y = 2.0 .* X .+ u
data = hcat(y, X, Z)
iv_moments(theta, d) = d[:, 3:5] .* (d[:, 1] .- d[:, 2] .* theta[1])
m = estimate_gmm(iv_moments, [0.0], data; weighting=:two_step,
                 X=reshape(X, :, 1), Z=Z)
report(m)
```

# See also

* [Simulation (DGPs)](dgp.md) - `dgp_gmm` designs with heteroskedastic and invalid-instrument arms
* [Shared Kernel](core.md) - HAC covariance behind the optimal weighting matrix
* [Local Projections](../multivariate/lp.md) - LP estimators including the GMM route
* [Panel VAR](../panel/pvar.md) - panel consumer of the linear GMM utilities
* [DSGE Models](../dsge/dsge.md) - DSGE estimation with `method=:smm`
