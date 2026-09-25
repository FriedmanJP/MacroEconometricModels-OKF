---
type: Feature
title: Multivariate GARCH (CCC / DCC / BEKK)
description: Conditional covariance modelling for return panels via constant conditional correlation, dynamic conditional correlation, and scalar/diagonal BEKK estimators.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/mgarch
tags:
  - mgarch
  - volatility
  - multivariate
  - time-series
  - forecasting
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:16:20Z
sources:
  - id: volatility-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/volatility.md
    title: Volatility Models docs page (Multivariate GARCH section)
  - id: api-multivariate
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/api/multivariate.md
    title: Multivariate Models API reference
  - id: src-mgarch
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/mgarch
    title: src/mgarch module source
---

# Summary

The `mgarch` module fits the conditional covariance matrix `H_t` of an `n`-dimensional return panel through three specifications.
CCC (Bollerslev 1990) reuses univariate `estimate_garch` margins and holds the standardized-residual correlation fixed (`H_t = D_t R D_t`).
DCC (Engle 2002) keeps the two-step margins and adds scalar correlation dynamics `(a, b)` estimated by correlation-targeted quasi-likelihood, with the Aielli (2013) cDCC correction available.
Scalar and diagonal BEKK (Engle & Kroner 1995) model the covariance directly with covariance targeting and no separate margins.
All three return an `MGARCHModel` with the `n x n x T` covariance path, plus `covariances` / `correlations` / `variances` accessors and multi-step covariance forecasts.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_ccc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/ccc.jl) | `estimate_ccc(Y::AbstractMatrix; p::Int=1, q::Int=1)` | CCC-GARCH with univariate GARCH margins and constant correlation; returns `MGARCHModel` |
| [estimate_dcc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/dcc.jl) | `estimate_dcc(Y::AbstractMatrix; p::Int=1, q::Int=1, correction=:none)` | DCC-GARCH with scalar `(a, b)` correlation dynamics; `correction=:aielli` gives cDCC |
| [estimate_bekk](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/bekk.jl) | `estimate_bekk(Y::AbstractMatrix; kind=:scalar)` | Scalar (`:scalar`) or diagonal (`:diagonal`) BEKK(1,1) with covariance targeting |
| [covariances](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/types.jl) | `covariances(m::MGARCHModel)` | Conditional covariance path `H_t` (`n x n x T`) |
| [correlations](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/types.jl) | `correlations(m::MGARCHModel)` | Conditional correlation path (`n x n x T`; constant models broadcast) |
| [variances](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/types.jl) | `variances(m::MGARCHModel)` | Per-series conditional variances (`T x n`, the diagonals of `H_t`) |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/forecast.jl) | `forecast(m::MGARCHModel, h::Int)` | `h`-step-ahead covariance forecasts as an `n x n x h` array |
| [MGARCHModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/mgarch/types.jl) | `struct MGARCHModel{T} <: AbstractMGARCHModel` | Fitted model: margins, `H`/`R`/`Rbar` paths, second-stage params, QML sandwich vcov |

# Examples

```julia
using MacroEconometricModels
ccc = estimate_ccc(Yret)
report(ccc)
dcc = estimate_dcc(Yret)
a, b = coef(dcc)
Rt = correlations(dcc)
fc = forecast(dcc, 10)
bekk = estimate_bekk(Yret)
report(bekk)
```

# See also

* [Vector Autoregression](/multivariate/var.md) - conditional-mean dynamics for the same return panels
* [Bayesian VAR](/multivariate/bvar.md) - TVP-VAR with stochastic volatility as a parameter-driven alternative
* [Statistical Identification](/multivariate/nongaussian.md) - GARCH-based SVAR identification via heteroskedasticity
* [Factor Models](/multivariate/factor.md) - dimension reduction for large cross-asset panels
