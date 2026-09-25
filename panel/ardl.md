---
type: Feature
title: ARDL, NARDL, and Panel ARDL
description: OLS ARDL estimation with long-run multipliers, PSS bounds testing, asymmetric NARDL, and PMG/MG/DFE panel estimators.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/ardl
tags:
  - ardl
  - nardl
  - cointegration
  - bounds-test
  - pmg
  - panel
status: approved
reviewed_by: chung9207
reviewed_at: 2026-09-25
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/panel
  at: 2026-09-25T01:19:04Z
sources:
  - id: ardl-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/ardl.md
    title: ARDL and bounds testing documentation page
  - id: src-ardl
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/ardl
    title: src/ardl module source
---

# Summary

The `ardl` module estimates autoregressive distributed-lag models by OLS for mixed `I(0)`/`I(1)` cointegration analysis.
`estimate_ardl` fits ARDL(p, q) with fixed or AIC/BIC grid-searched lag orders and PSS deterministic cases I-V; `long_run` recovers the long-run multipliers `theta` with analytic delta-method SEs, and `ecm_form` gives the conditional error-correction re-parameterization with speed of adjustment `alpha`.
`bounds_test` implements the Pesaran-Shin-Smith bounds test, reporting the non-standard `F`-statistic on the level block and the `t`-statistic on the lagged dependent level against tabulated `I(0)`/`I(1)` bounds (no p-values).
`estimate_nardl` splits regressors into positive/negative partial sums for asymmetric cointegration (Shin-Yu-Greenwood-Nimmo), with `symmetry_test` (long- and short-run Wald tests) and `dynamic_multipliers` with bootstrap bands.
`estimate_pmg` fits dynamic heterogeneous panels via Pooled Mean Group, Mean Group, and Dynamic Fixed Effects, with a `hausman_test` for long-run homogeneity.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_ardl](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/estimation.jl) | `estimate_ardl(y::AbstractVector, X::AbstractMatrix; p=:auto, q=:auto, max_p=4, max_q=4, ic=:aic, case=3, trend=:none, xnames=nothing, yname="y")` | OLS ARDL(p,q) with optional IC lag selection |
| [long_run](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/estimation.jl) | `long_run(m::ARDLModel)` | Long-run multipliers with delta-method SEs |
| [long_run](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/nardl.jl) | `long_run(m::NARDLModel)` | Long-run multipliers for the partial-sum design |
| [ecm_form](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/estimation.jl) | `ecm_form(m::ARDLModel)` | Conditional EC form: speed of adjustment and levels term |
| [bounds_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/bounds.jl) | `bounds_test(m::ARDLModel; case=m.case, level=0.05, cv_source=:pss)` | PSS bounds F/t test for a level relationship |
| [estimate_nardl](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/nardl.jl) | `estimate_nardl(y::AbstractVector, X::AbstractMatrix; asymmetric=:all, p=:auto, q=:auto, max_p=4, max_q=4, ic=:aic, case=3, ...)` | Asymmetric ARDL on positive/negative partial sums |
| [symmetry_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/nardl.jl) | `symmetry_test(m::NARDLModel)` | Per-regressor long-run and short-run symmetry Wald tests |
| [dynamic_multipliers](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/nardl.jl) | `dynamic_multipliers(m::NARDLModel, H::Int; bootstrap=true, nreps=500, level=0.95, seed=nothing, rng)` | Cumulative dynamic multipliers with bootstrap bands |
| [estimate_pmg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/pmg.jl) | `estimate_pmg(pd::PanelData, y::Symbol, xs::Symbol...; p=1, q=1, method=:pmg, trend=:constant, maxiter=100, tol=1e-8)` | Panel ARDL: `:pmg`, `:mg`, or `:dfe` |
| [estimate_pmg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/pmg.jl) | `estimate_pmg(y::AbstractVector, X::AbstractMatrix, id, time; xnames=nothing, yname="y", kwargs...)` | Panel ARDL from raw vectors (builds `PanelData` internally) |
| [hausman_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/pmg.jl) | `hausman_test(efficient::PMGModel, consistent::PMGModel)` | PMG vs MG test of long-run homogeneity |
| [ARDLModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct ARDLModel{T}` | Fitted ARDL: OLS block, lag bookkeeping, cached long-run |
| [ARDLLongRun](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct ARDLLongRun{T}` | Long-run multipliers, SEs, denominator |
| [ARDLBoundsTest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct ARDLBoundsTest{T}` | Bounds F/t statistics, CV tables, decisions |
| [NARDLModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct NARDLModel{T}` | Fitted NARDL wrapping the enlarged ARDL design |
| [NARDLSymmetryTest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct NARDLSymmetryTest{T}` | Long- and short-run symmetry Wald statistics |
| [NARDLMultipliers](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct NARDLMultipliers{T}` | Cumulative multipliers with bootstrap bands |
| [PMGModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ardl/types.jl) | `struct PMGModel{T}` | Panel ARDL fit: common/unit long-run, speeds of adjustment |

# Examples

```julia
using MacroEconometricModels
m = estimate_ardl(y, x; p=1, q=1, case=3)
report(m)
lr = long_run(m)          # theta with delta-method SEs
bt = bounds_test(m; level=0.05)
report(bt)
nm = estimate_nardl(y, x; asymmetric=:all, p=1, q=1, case=3)
st = symmetry_test(nm)
```

# See also

* [Panel Regression](preg.md) - static and dynamic linear panel estimators
* [Panel VAR](pvar.md) - multivariate dynamic panels
* [Difference-in-Differences](did.md) - treatment-effect designs on panels
* [Cointegrating Regression](/multivariate/cointreg.md) - FMOLS/CCR/DOLS single-equation cointegration
* [Vector Error Correction Models](/multivariate/vecm.md) - system cointegration with multiple vectors
