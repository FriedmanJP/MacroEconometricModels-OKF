---
type: Feature
title: Systems of Equations (SUR and 3SLS)
description: Joint estimation of multi-equation systems by seemingly-unrelated regressions (SUR) and three-stage least squares (3SLS).
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/system
tags:
  - cross-section
  - systems
  - sur
  - 3sls
  - simultaneous-equations
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/cross-section
  at: 2026-09-25T01:30:07Z
sources:
  - id: regression-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/regression.md
    title: Linear Regression docs page (Systems of Equations section)
  - id: src-system
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/system
    title: src/system module source
---

# Summary

The `system` module jointly estimates multi-equation systems whose errors are correlated across equations. `estimate_sur` fits Zellner's (1962) seemingly-unrelated regressions by feasible GLS, stacking the `M` equations as `y = X*beta + u` with `Cov(u) = Sigma (x) I` and estimating the residual cross-covariance from equation-by-equation OLS residuals with the classical Zellner divisor `T`; `estimate_3sls` fits the Zellner-Theil (1962) three-stage least squares estimator for simultaneous systems, projecting each equation's regressors onto an instrument space before the system GLS step. SUR supports iterated FGLS to the Gaussian MLE (`iterate=true`) and linear cross-equation restrictions `R*vec(B) = r` via restricted GLS; 3SLS accepts one shared instrument matrix or per-equation matrices. Both return typed results (`SURModel`, `ThreeSLSModel`) carrying per-equation coefficients, the full system covariance, `Sigma`, and McElroy (1977) system R-squared, all printable via `report()`; only `SURModel` carries the Gaussian system log-likelihood (`loglik`) plus iteration/restriction flags, while `ThreeSLSModel` records per-equation instrument counts (`n_instruments`).

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_sur](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/system/sur.jl) | `estimate_sur(eqs; iterate=false, tol=1e-8, maxiter=100, restrict=nothing, eqnames=nothing)` | Zellner SUR by FGLS; `eqs` is a vector of `(y, X)` or `(y, X, names)` tuples |
| [estimate_3sls](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/system/threesls.jl) | `estimate_3sls(eqs, Z; instruments=:common, eqnames=nothing)` | Zellner-Theil 3SLS with `:common` or `:perequation` instruments |
| [SURModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/system/types.jl) | `struct SURModel{T<:AbstractFloat}` | Fitted SUR: per-equation `betas`/`ses`, system `vcov_mat`, `Sigma`, `det_sigma`, `mcelroy_r2`, `loglik`, iteration flags |
| [ThreeSLSModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/system/types.jl) | `struct ThreeSLSModel{T<:AbstractFloat}` | Fitted 3SLS: shared system fields plus `n_instruments`, but no `loglik` or iteration flags |

# Examples

```julia
using MacroEconometricModels
pd = load_example(:grunfeld)
ge = group_data(pd, "General Electric")
wh = group_data(pd, "Westinghouse")
Xge = hcat(ones(20), ge.data[:, 2], ge.data[:, 3])
Xwh = hcat(ones(20), wh.data[:, 2], wh.data[:, 3])
cols = ["const", "value", "capital"]
m = estimate_sur([(ge.data[:, 1], Xge, cols), (wh.data[:, 1], Xwh, cols)]; eqnames=["GE", "Westinghouse"])
report(m)
```

# See also

* [Regression (OLS, IV, and Limited Dependent Variables)](/cross-section/reg.md) - single-equation OLS/IV foundation the system estimators build on
* [Panel Regression](/panel/preg.md) - fixed/random-effects and panel IV estimators for longitudinal data
* [Vector Autoregression (VAR)](/multivariate/var.md) - multi-equation systems in the time-series domain
