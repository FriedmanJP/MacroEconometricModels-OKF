---
type: Feature
title: Bayesian VAR (BVAR)
description: Minnesota-prior Bayesian VAR estimation with marginal-likelihood hyperparameter selection, posterior sampling, and TVP/MF extensions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/bvar
tags:
  - bvar
  - bayesian
  - multivariate
  - time-series
  - shrinkage
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:11:32Z
sources:
  - id: bvar-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/bayesian.md
    title: Bayesian VAR docs page
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-bvar
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/bvar
    title: src/bvar module source
---

# Summary

The `bvar` module estimates VARs with the Minnesota prior (Litterman 1986) implemented via dummy observations, conjugate Normal-Inverse-Wishart posteriors, and data-driven hyperparameter selection through the closed-form marginal likelihood (Giannone, Lenza & Primiceri 2015).
`estimate_bvar` samples the posterior either i.i.d. from the analytical posterior (`:direct`) or with a two-block Gibbs sampler (`:gibbs`); `irf`, `fevd`, `historical_decomposition`, and `forecast` then propagate full parameter uncertainty into credible bands.
Extensions cover time-varying parameters with stochastic volatility (`estimate_tvpvar`) and mixed-frequency panels (`estimate_mfvar`).
Scale data to percent-like units before estimating: the prior scale is not unit-invariant.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_bvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/estimation.jl) | `estimate_bvar(Y, p; n_draws=1000, sampler=:direct, burnin=0, thin=1, prior=:normal, hyper=nothing, hyperopt=:glp, varnames=nothing, seed=nothing, rng)` | Posterior sampling under diffuse or Minnesota prior; returns `BVARPosterior` |
| [gen_dummy_obs](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/priors.jl) | `gen_dummy_obs(Y, p, hyper::MinnesotaHyperparameters)` | Minnesota prior dummy observations |
| [log_marginal_likelihood](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/priors.jl) | `log_marginal_likelihood(Y, p, hyper::MinnesotaHyperparameters)` | Closed-form marginal likelihood for hyperparameter choice |
| [optimize_hyperparameters](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/priors.jl) | `optimize_hyperparameters(Y, p; grid_size=20, tau_range=(0.01, 10.0))` | Tau-only grid search on the marginal likelihood |
| [optimize_hyperparameters_full](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/priors.jl) | `optimize_hyperparameters_full(Y, p; tau_grid, lambda_grid, mu_grid)` | Joint grid over tau, lambda, and mu for large systems |
| [optimize_hyperparameters_glp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/glp.jl) | `optimize_hyperparameters_glp(Y, p; decay=0.5, omega=1.0, starts=4, max_iter=500, f_reltol=1e-8, verbose=true)` | GLP (2015) joint optimization with Gamma hyperpriors |
| [posterior_mean_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/estimation.jl) | `posterior_mean_model(post::BVARPosterior; data)` | Posterior-mean summary as a `VARModel` |
| [posterior_median_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/estimation.jl) | `posterior_median_model(post::BVARPosterior; data)` | Posterior-median summary as a `VARModel` |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/estimation.jl) | `forecast(post::BVARPosterior, h::Int; reps=nothing, conf_level=0.95, point_estimate=:mean, store_draws=false, rng)` | Predictive simulation with credible intervals |
| [conditional_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/conditional_forecast.jl) | `conditional_forecast(post::BVARPosterior, conditions, h::Int; ...)` | Conditional forecast over posterior draws |
| [estimate_tvpvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/tvpvar.jl) | `estimate_tvpvar(Y, p; tvp=true, sv=true, n_draws=2000, n_burn=1000, thin=1, n_train=0, k_Q=0.01, k_S=0.1, k_W=0.01, varnames, seed=nothing, rng)` | TVP-VAR with stochastic volatility via Gibbs sampling |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/tvpvar.jl) | `irf(post::TVPVARPosterior, horizon::Int; t=post.T_eff, n_draws=500, quantile_levels, stationary_only=true)` | Time-`t` impulse responses from TVP draws |
| [volatility_path](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/tvpvar.jl) | `volatility_path(post::TVPVARPosterior; quantile_levels=[0.16, 0.5, 0.84])` | Posterior volatility path with quantiles |
| [estimate_mfvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/mfvar.jl) | `estimate_mfvar(data, p; low_freq=Int[], freq_ratio=3, aggregation=:growth, n_draws=1000, n_burn=500, prior=:minnesota, hyper=nothing, varnames, seed=nothing, rng)` | Mixed-frequency VAR with latent high-frequency states |
| [latent_path](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/mfvar.jl) | `latent_path(post::MFVARPosterior; quantile_levels=[0.16, 0.5, 0.84])` | Posterior latent high-frequency path with quantiles |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/mfvar.jl) | `irf(post::MFVARPosterior, horizon::Int; kwargs...)` | Impulse responses via the latent-state BVAR |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/mfvar.jl) | `forecast(post::MFVARPosterior, h::Int; kwargs...)` | Forecasts via the latent-state BVAR |
| [BVARPosterior](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/types.jl) | `struct BVARPosterior{T}` | Posterior draws of `(B, Sigma)` with prior metadata |
| [BVARForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/types.jl) | `struct BVARForecast{T}` | Predictive draws with credible bands |
| [TVPVARPosterior](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/tvpvar.jl) | `struct TVPVARPosterior{T}` | TVP coefficient and volatility draws |
| [MFVARPosterior](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/mfvar.jl) | `struct MFVARPosterior{T}` | Mixed-frequency posterior with latent states |
| [GLPHyperparameters](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/bvar/glp.jl) | `struct GLPHyperparameters{T}` | Jointly optimized GLP hyperparameters |
| [MinnesotaHyperparameters](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/var/types.jl) | `MinnesotaHyperparameters(; tau=3.0, decay=0.5, ...)` | Minnesota tightness, lag decay, cross-variable shrinkage |

# Examples

```julia
using MacroEconometricModels
post = estimate_bvar(Y, 2; n_draws=100, prior=:minnesota,
                     varnames=["INDPRO", "CPI", "FFR"])
report(post)
birf = irf(post, 20; method=:cholesky)
report(birf)
fc = forecast(post, 12; conf_level=0.95)
report(fc)
```

# See also

* [Vector Autoregression](var.md) - frequentist counterpart and summary-model type
* [Vector Error Correction Models](vecm.md) - cointegrated systems
* [Local Projections](lp.md) - non-Bayesian direct impulse-response estimation
* [Factor Models](factor.md) - dimension reduction for large Bayesian systems
