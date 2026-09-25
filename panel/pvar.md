---
type: Feature
title: Panel VAR (PVAR)
description: GMM and fixed-effects estimation of panel VARs with IRFs, FEVD, stability diagnostics, and bootstrap inference.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/pvar
tags:
  - pvar
  - panel
  - dynamic-panel
  - gmm
  - irf
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/panel
  at: 2026-09-25T01:19:04Z
sources:
  - id: pvar-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/pvar.md
    title: Panel VAR documentation page
  - id: src-pvar
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/pvar
    title: src/pvar module source
  - id: src-pvar-tests
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/teststat
    title: PVAR specification tests (Hansen J, MMSC, lag selection)
---

# Summary

The `pvar` module estimates Panel VAR(p) models `y_it = mu_i + A_1 y_{i,t-1} + ... + A_p y_{i,t-p} + e_it` on `PanelData`.
`estimate_pvar` removes the entity fixed effect by first-differencing (`:fd`) or forward orthogonal deviations (`:fod`, Arellano-Bover 1995) and estimates by GMM with lagged levels as instruments (Arellano-Bond 1991), optionally adding level equations instrumented by lagged differences (Blundell-Bond system GMM), with Windmeijer-corrected two-step standard errors.
`estimate_pvar_feols` is the within (demeaning) OLS alternative with group-clustered standard errors, consistent for large `T` but subject to Nickell bias when `T` is small.
Structural analysis covers Cholesky orthogonalized IRFs, order-free generalized IRFs (Pesaran-Shin 1998), FEVD, companion-matrix stability checks, and a group-level block bootstrap for IRF confidence bands.
Specification testing covers the Hansen J-test, Andrews-Lu MMSC, and MMSC-based lag selection (implemented in `src/teststat`).

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_pvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/estimation.jl) | `estimate_pvar(d::PanelData, p::Int; dependent_vars, predet_vars, exog_vars, transformation=:fd, steps=:twostep, system_instruments=false, collapse=false, min_lag_endo=2, max_lag_endo=99, ...)` | FD or system GMM estimation of Panel VAR(p); returns `PVARModel` |
| [estimate_pvar_feols](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/estimation.jl) | `estimate_pvar_feols(d::PanelData, p::Int; dependent_vars, predet_vars, exog_vars)` | Within (FE-OLS) estimator with cluster-robust SEs |
| [pvar_oirf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_oirf(model::PVARModel, H::Int)` | Cholesky orthogonalized IRFs, `(H+1) x m x m` |
| [pvar_girf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_girf(model::PVARModel, H::Int)` | Generalized (ordering-free) IRFs |
| [pvar_fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_fevd(model::PVARModel, H::Int)` | Forecast error variance decomposition |
| [pvar_stability](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_stability(model::PVARModel)` | Companion eigenvalues/moduli and stability verdict |
| [pvar_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_irf(model::PVARModel, H::Int; irf_type=:oirf, ci=nothing)` | Name-carrying IRF wrapper for plotting |
| [pvar_fevd_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/analysis.jl) | `pvar_fevd_result(model::PVARModel, H::Int)` | Name-carrying FEVD wrapper for plotting |
| [pvar_bootstrap_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/bootstrap.jl) | `pvar_bootstrap_irf(model::PVARModel, H::Int; irf_type=:oirf, n_draws=500, ci=0.95, rng)` | Group-level block bootstrap IRFs with quantile CIs |
| [pvar_hansen_j](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_hansen_j.jl) | `pvar_hansen_j(model::PVARModel)` | Hansen J-test of overidentifying restrictions |
| [pvar_mmsc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_mmsc.jl) | `pvar_mmsc(model::PVARModel; hq_criterion=2.1)` | Andrews-Lu MMSC (BIC/AIC/HQIC) for a fitted model |
| [pvar_lag_selection](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_lag_selection.jl) | `pvar_lag_selection(d::PanelData, max_p::Int; dependent_vars, kwargs...)` | MMSC comparison across lag orders with best-lag picks |
| [PVARModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/types.jl) | `struct PVARModel{T} <: StatsAPI.RegressionModel` | Fitted PVAR: `Phi`, `Sigma`, `se`, instruments, diagnostics |
| [PVARStability](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/types.jl) | `struct PVARStability` | Eigenvalues, moduli, `is_stable` |
| [PVARTestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/pvar/types.jl) | `struct PVARTestResult` | J/MMSC test result container |

# Examples

```julia
using MacroEconometricModels
pd = apply_tcode(load_example(:pwt), 5)   # growth rates for stationarity
dep_vars = ["rgdpna", "emp", "hc"]
model = estimate_pvar(pd, 1; dependent_vars=dep_vars, steps=:twostep,
                      collapse=true, max_lag_endo=6)
report(model)
j = pvar_hansen_j(model)                  # instrument validity
irfs = pvar_oirf(model, 10)               # (H+1) x m x m, [horizon, response, shock]
decomp = pvar_fevd(model, 10)
```

# See also

* [Panel Regression](preg.md) - single-equation FE/RE/IV and Arellano-Bond estimators
* [Difference-in-Differences](did.md) - treatment-effect designs on panels
* [ARDL & Panel ARDL](ardl.md) - single-equation dynamics and PMG/MG/DFE
* [Vector Autoregression](../multivariate/var.md) - time-series VAR without the panel dimension
* [Panel Tests](../testing/teststat.md) - panel unit-root pretests before PVAR estimation
* [GMM/SMM](../infrastructure/gmm.md) - one-step/optimal/two-step GMM machinery behind Arellano-Bond and system estimation
