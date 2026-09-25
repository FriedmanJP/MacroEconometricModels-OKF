---
type: Package
title: MacroEconometricModels.jl
description: A comprehensive Julia package for macroeconometric research and analysis.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl
tags: [macroeconometrics, julia, time-series, dsge, forecasting]
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/skeleton
  at: 2026-09-25T01:03:00Z
sources:
  - id: upstream-readme
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/README.md
    title: Upstream README (feature summary and installation)
  - id: upstream-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/docs/src
    title: Upstream documentation pages
  - id: upstream-src
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src
    title: Upstream source tree
---

# Summary

MacroEconometricModels.jl is a comprehensive Julia package for
macroeconometric research and analysis. It covers univariate time-series
models, nonlinear and state-space models, multivariate time series, panel
methods, DSGE (including heterogeneous-agent, continuous-time, and OLG
variants), input-output analysis, policy counterfactuals,
cross-sectional and nonparametric methods, forecasting, and a broad
suite of specification and hypothesis tests.

# Feature map

| Domain | Contents |
|---|---|
| [Univariate](/univariate/) | ARIMA, ARCH/GARCH, stochastic volatility, filters, X-13, spectral analysis |
| [Nonlinear & state-space](/nonlinear-statespace/) | Threshold/STAR/Markov-switching, state-space, TVP regression |
| [Multivariate](/multivariate/) | VAR, VECM, BVAR, local projections, factor models, MGARCH |
| [Panel](/panel/) | Panel VAR, panel regression/IV, panel ARDL, DiD, event studies |
| [DSGE](/dsge/) | Model specification, solvers, estimation, HA/CT models, OLG |
| [Input-output](/io/) | IO tables, multipliers, linkages, decomposition, production networks |
| [Policy counterfactuals](/policy-counterfactuals/) | Rule counterfactuals, OPP, model banks, menus |
| [Cross-section](/cross-section/) | OLS/IV, penalized/robust regression, LDV, discrete choice |
| [Nonparametric](/nonparametric/) | KDE, kernel/local-polynomial regression, LOWESS |
| [Forecasting](/forecasting/) | Forecasting, forecast evaluation/combination, nowcasting |
| [Testing](/testing/) | Unit-root, cointegration, break, and diagnostic tests |
| [Infrastructure](/infrastructure/) | Core utilities, data, DGP simulation, GMM, plotting |

# Usage

Install from the Julia package registry (per the upstream README):

```julia
using Pkg
Pkg.add("MacroEconometricModels")
```

Then pick a domain from the feature map above and open its concept for
the estimators, function tables, and worked examples.
