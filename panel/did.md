---
type: Feature
title: Difference-in-Differences and Event Study LP
description: Staggered-adoption DiD with heterogeneity-robust estimators, Bacon/negative-weight diagnostics, HonestDiD sensitivity, and LP event-study estimators.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/did
tags:
  - did
  - event-study
  - staggered-adoption
  - causal
  - lp-did
  - honest-did
status: approved
reviewed_by: chung9207
reviewed_at: 2026-09-25
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/panel
  at: 2026-09-25T01:19:04Z
sources:
  - id: did-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/did.md
    title: Difference-in-Differences documentation page
  - id: event-study-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/event_study.md
    title: Event Study LP documentation page
  - id: src-did
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/did
    title: src/did module source
---

# Summary

The `did` module covers static and event-time treatment effects under staggered adoption, plus the local-projection formulation of the same designs.
`estimate_did` dispatches on `method`: traditional TWFE event-study regression plus four heterogeneity-robust estimators — Callaway-Sant'Anna group-time ATTs (with `:never_treated`/`:not_yet_treated` controls and `:varying`/`:universal` base periods), the Sun-Abraham interaction-weighted estimator, the Borusyak-Jaravel-Spiess imputation estimator, and the de Chaisemartin-D'Haultfoeuille first-difference estimator with block-bootstrap SEs.
Diagnostics include the Goodman-Bacon 2x2 decomposition, a joint pre-trend Wald test, the dCDH negative-weight check, and Rambachan-Roth HonestDiD sensitivity analysis under relative-magnitude or smoothness restrictions.
`estimate_event_study_lp` and `estimate_lp_did` run horizon-by-horizon local projections on long-differenced outcomes with a switching-indicator treatment; LP-DiD adds clean-control-sample rules (absorbing/non-absorbing/one-off), pre-mean differencing, IPW reweighting, and pooled estimates, matching Stata `lpdid`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_did](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/estimation.jl) | `estimate_did(pd::PanelData, outcome, treatment; method=:twfe, leads=0, horizon=5, control_group=:never_treated, cluster=:unit, base_period=:varying, n_boot=200, ...)` | TWFE / Callaway-Sant'Anna / Sun-Abraham / BJS / dCDH event-study ATTs |
| [estimate_event_study_lp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/event_study.jl) | `estimate_event_study_lp(pd::PanelData, outcome, treatment, H::Int; leads=3, lags=4, covariates=String[], cluster=:unit, conf_level=0.95)` | Horizon-by-horizon LP event study with switching indicator |
| [estimate_lp_did](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/lpdid.jl) | `estimate_lp_did(pd::PanelData, outcome, treatment, H::Int; pre_window=3, ylags=0, dylags=0, nonabsorbing=nothing, oneoff=false, pmd=nothing, reweight=false, ...)` | LP-DiD with clean control samples, PMD, IPW, pooled estimates |
| [bacon_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/diagnostics.jl) | `bacon_decomposition(pd::PanelData, outcome, treatment)` | Goodman-Bacon 2x2 decomposition of static TWFE |
| [pretrend_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/diagnostics.jl) | `pretrend_test(result::DIDResult)` | Joint Wald test of pre-treatment coefficients |
| [pretrend_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/diagnostics.jl) | `pretrend_test(result::EventStudyLP)` | Pre-trend Wald test for LP event-study results |
| [negative_weight_check](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/diagnostics.jl) | `negative_weight_check(pd::PanelData, treatment)` | dCDH check for negative TWFE weights on ATT cells |
| [honest_did](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/honest_did.jl) | `honest_did(result::Union{DIDResult, EventStudyLP}; restriction=:rm, Mbar=1.0, M=0.0, conf_level=0.95)` | Rambachan-Roth robust CIs and breakdown values |
| [honest_did](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/honest_did.jl) | `honest_did(betahat::AbstractVector, sigma::AbstractMatrix; num_pre, num_post, restriction=:rm, Mbar=1.0, M=0.0, l_vec=nothing, ...)` | Core HonestDiD on raw event-study coefficients + covariance |
| [DIDResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct DIDResult{T} <: AbstractFrequentistResult` | Event-time ATTs, group-time matrix, overall ATT, `att_vcov` |
| [EventStudyLP](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct EventStudyLP{T}` | Per-horizon LP coefficients, SEs, `T_eff` |
| [LPDiDResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct LPDiDResult{T}` | LP-DiD coefficients with pooled estimates and CCS spec |
| [BaconDecomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct BaconDecomposition{T}` | 2x2 estimates, weights, comparison types |
| [PretrendTestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct PretrendTestResult{T}` | Pre-trend Wald statistic, p-value, df |
| [NegativeWeightResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct NegativeWeightResult{T}` | TWFE weights with negative-weight flags |
| [HonestDiDResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/did/types.jl) | `struct HonestDiDResult{T}` | Robust vs conventional CIs, breakdown value |

# Examples

```julia
using MacroEconometricModels
pd = load_example(:mpdta)   # county minimum-wage panel; first_treat = adoption year
did_cs = estimate_did(pd, "lemp", "first_treat"; method=:callaway_santanna,
                      leads=3, horizon=3, control_group=:never_treated)
report(did_cs)
pt = pretrend_test(did_cs)
h = honest_did(did_cs; Mbar=1.0, conf_level=0.95)
report(h)
```

# See also

* [Panel Regression](preg.md) - panel models identified from covariates
* [Panel VAR](pvar.md) - multivariate dynamic panels
* [ARDL & Panel ARDL](ardl.md) - single-equation dynamics and PMG/MG/DFE
* [Local Projections](/multivariate/lp.md) - time-series LP impulse responses
* [Panel Tests](/testing/teststat.md) - panel unit-root pretests
