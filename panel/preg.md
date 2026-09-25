---
type: Feature
title: Panel Regression (xtreg/xtivreg/xtlogit/xtprobit)
description: Linear, IV, and discrete-choice panel estimators with HDFE absorption, specification tests, and robust covariances.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/preg
tags:
  - panel
  - fixed-effects
  - random-effects
  - iv
  - logit
  - probit
  - hdfe
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/panel
  at: 2026-09-25T01:19:04Z
sources:
  - id: panel-reg-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/panel_reg.md
    title: Panel Regression documentation page
  - id: src-preg
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/preg
    title: src/preg module source
---

# Summary

The `preg` module follows Stata `xtreg`/`xtivreg`/`xtlogit`/`xtprobit` conventions.
`estimate_xtreg` fits fixed effects, Swamy-Arora random effects, first-difference, between, Mundlak CRE, and the Arellano-Bond (`:ab`) / Blundell-Bond (`:bb`) dynamic-panel GMM estimators (with Windmeijer-corrected covariances and AR/Hansen diagnostics), plus two-way FE and arbitrary-dimension HDFE absorption by alternating projections (Guimaraes-Portugal/Correia), Prais-Winsten AR(1) FGLS, and five covariance estimators (`:ols`, `:cluster`, `:twoway`, `:driscoll_kraay`, `:pcse`).
`estimate_xtiv` covers FE-IV, Baltagi EC2SLS RE-IV, FD-IV, and Hausman-Taylor with first-stage F, Cragg-Donald/Kleibergen-Paap weak-instrument statistics, and Sargan-Hansen J.
`estimate_xtlogit` (pooled/FE-conditional/RE/CRE) and `estimate_xtprobit` (pooled/RE/CRE, no FE) cover panel discrete choice, with `marginal_effects` for average marginal effects.
Six specification tests (Hausman, Breusch-Pagan LM, F-test for FE, Pesaran CD, Wooldridge AR, modified Wald) guide estimator choice.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_xtreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/estimation.jl) | `estimate_xtreg(pd::PanelData, depvar::Symbol, indepvars::Vector{Symbol}; model=:fe, twoway=false, absorb=Symbol[], cov_type=:cluster, ar1=:none, ...)` | Linear panel: `:fe`, `:re`, `:fd`, `:between`, `:cre`, `:ab`, `:bb` |
| [estimate_xtiv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/iv.jl) | `estimate_xtiv(pd::PanelData, depvar::Symbol, exog::Vector{Symbol}, endog::Vector{Symbol}; instruments=Symbol[], model=:fe, cov_type=:cluster, ...)` | Panel IV: `:fe`, `:re` (EC2SLS), `:fd`, `:hausman_taylor` |
| [estimate_xtlogit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/logit.jl) | `estimate_xtlogit(pd::PanelData, depvar::Symbol, indepvars::Vector{Symbol}; model=:pooled, cov_type=:cluster, maxiter=2000, tol=1e-8, n_quadrature=12)` | Panel logit: `:pooled`, `:fe` (conditional), `:re`, `:cre` |
| [estimate_xtprobit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/probit.jl) | `estimate_xtprobit(pd::PanelData, depvar::Symbol, indepvars::Vector{Symbol}; model=:pooled, cov_type=:cluster, maxiter=200, tol=1e-8, n_quadrature=12)` | Panel probit: `:pooled`, `:re`, `:cre` (no FE) |
| [absorb_fe](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/hdfe.jl) | `absorb_fe(y::AbstractVector, X::AbstractMatrix, fe_groups; tol=1e-8, maxiter=1000, accel=true)` | Standalone HDFE residualization by alternating projections |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/margins.jl) | `marginal_effects(m::Union{PanelLogitModel, PanelProbitModel}; conf_level=0.95, n_quadrature=12)` | Average marginal effects with delta-method SEs |
| [hausman_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `hausman_test(fe::PanelRegModel, re::PanelRegModel)` | FE vs RE specification test (generalized inverse) |
| [breusch_pagan_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `breusch_pagan_test(re::PanelRegModel)` | LM test for presence of random effects |
| [f_test_fe](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `f_test_fe(fe::PanelRegModel)` | Joint significance of entity fixed effects |
| [pesaran_cd_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `pesaran_cd_test(m::PanelRegModel)` | Cross-sectional dependence in residuals |
| [wooldridge_ar_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `wooldridge_ar_test(fe::PanelRegModel)` | AR(1) serial correlation in differenced residuals |
| [modified_wald_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/tests.jl) | `modified_wald_test(fe::PanelRegModel)` | Groupwise heteroskedasticity test |
| [arellano_bond_ar_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_ar_test.jl) | `arellano_bond_ar_test(m::PanelRegModel; order=2)` | AR(m) test read off `:ab`/`:bb` dynamic diagnostics |
| [PanelRegModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/types.jl) | `struct PanelRegModel{T} <: StatsAPI.RegressionModel` | Fitted linear panel model with R2s, variance components |
| [PanelIVModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/types.jl) | `struct PanelIVModel{T} <: StatsAPI.RegressionModel` | Fitted panel IV model with first-stage/Sargan diagnostics |
| [PanelLogitModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/types.jl) | `struct PanelLogitModel{T} <: StatsAPI.RegressionModel` | Fitted panel logit model |
| [PanelProbitModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/types.jl) | `struct PanelProbitModel{T} <: StatsAPI.RegressionModel` | Fitted panel probit model |
| [PanelTestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/preg/types.jl) | `struct PanelTestResult{T} <: StatsAPI.HypothesisTest` | Specification-test result container |

# Examples

```julia
using MacroEconometricModels
pd = xtset(df_pwt, :country, :year)
m_fe = estimate_xtreg(pd, :lngdppc, [:hc, :lnk])
m_re = estimate_xtreg(pd, :lngdppc, [:hc, :lnk]; model=:re)
ht = hausman_test(m_fe, m_re)
report(ht)
m_final = estimate_xtreg(pd, :lngdppc, [:hc, :lnk];
                         absorb=[:entity, :time], cov_type=:driscoll_kraay)
```

# See also

* [Panel VAR](pvar.md) - multivariate dynamic panels with GMM
* [Difference-in-Differences](did.md) - treatment-effect designs on panels
* [ARDL & Panel ARDL](ardl.md) - dynamic heterogeneous panels (PMG/MG/DFE)
* [Cross-Section Regression](/cross-section/reg.md) - non-panel OLS/IV/discrete choice
* [Panel Tests](/testing/teststat.md) - panel unit-root and cointegration pretests
