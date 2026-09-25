---
type: Feature
title: State-Space Models (Unobserved Components, TVP)
description: User-specified linear-Gaussian state-space models with MLE, Kalman filtering, RTS smoothing, local-level/trend wrappers, and TVP regression.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/statespace
tags:
  - state-space
  - kalman-filter
  - unobserved-components
  - tvp
  - structural-time-series
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/nonlinear-statespace
  at: 2026-09-25T01:09:05Z
sources:
  - id: statespace-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/statespace.md
    title: State-Space Models docs page
  - id: statespace-types
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/types.jl
    title: StateSpaceModel type and constructors
  - id: statespace-estimation
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl
    title: MLE, local-level/trend wrappers, TVP regression
  - id: statespace-api
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/api.jl
    title: State-space forecasting API
---

# Summary

The `statespace` module provides a general single-block linear-Gaussian state-space object with prediction-error-decomposition MLE of its hyper-parameters, Kalman filtering and RTS smoothing through the shared kernel, multi-step forecasting with predictive variances, and convenience wrappers for the local-level model, the local-linear-trend model, and time-varying-parameter (TVP) regression with a random-walk coefficient path.

# Functions

| Function | Signature | Role |
|---|---|---|
| [StateSpaceModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/types.jl) | `StateSpaceModel(Z, H, T, Q; d, c, R, a1, P1, init_mode=:kappa, kappa=1e6)` | Unfitted spec from fixed system matrices; `a1`/`P1` must be supplied together |
| [StateSpaceModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/types.jl) | `StateSpaceModel(build, θ0; init_mode=:kappa, kappa=1e6, param_names)` | Unfitted parametric spec carrying a `build(θ)` closure for MLE |
| [estimate_statespace](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl) | `estimate_statespace(build, θ0, y; init_mode, kappa, param_names, theta_transform, display_names, iterations, g_tol)` | L-BFGS MLE of a builder-defined system plus filtering and smoothing |
| [estimate_statespace](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl) | `estimate_statespace(ss::StateSpaceModel, y)` | Filter and smooth a fixed-matrix spec without optimization |
| [local_level](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl) | `local_level(y; init_mode=:kappa, kappa=1e6)` | Random-walk-plus-noise MLE; reports σ²_ε and σ²_η |
| [local_linear_trend](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl) | `local_linear_trend(y; init_mode=:kappa, kappa=1e6)` | Stochastic level-plus-slope MLE; reports σ²_ε, σ²_ξ, σ²_ζ |
| [estimate_tvp_reg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/estimation.jl) | `estimate_tvp_reg(y, X; intercept=true, init_mode=:kappa, kappa=1e6, iterations, g_tol)` | Random-walk-coefficient regression; smoothed coefficient paths in `smoothed_state` |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/statespace/api.jl) | `forecast(ss::StateSpaceModel, h::Integer)` | h-step forecast via the state recursion with predictive covariances |

# Examples

```julia
using MacroEconometricModels

# Local-level (random walk + noise) on the Nile series
nile = load_example(:nile)
m = local_level(nile)
report(m)

# Five-step forecast with predictive standard errors
fc = forecast(m, 5)
[fc.mean fc.se]
```

# See also

- /nonlinear-statespace/nonlinear.md
- /univariate/arima.md
- /forecasting/forecasting.md
- /multivariate/var.md
