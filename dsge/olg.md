---
type: Feature
title: Overlapping Generations
description: Blanchard (1985) perpetual-youth steady state and saddle-path dynamics plus Auerbach-Kotlikoff life-cycle EGM with retirement, pensions, and perfect-foresight transitions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/olg
tags:
  - dsge
  - olg
  - life-cycle
  - demographics
  - pensions
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/dsge
  at: 2026-09-25T01:22:02Z
sources:
  - id: dsge-olg
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_olg.md
    title: Overlapping Generations page
  - id: dsge-heterogeneity
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_heterogeneity.md
    title: Heterogeneity and Continuous Time sub-hub page
  - id: src-olg
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/olg
    title: src/olg module source
---

# Summary

The `olg` module replaces the infinitely-lived household with finite horizons in two flavors.
`BlanchardOLG` is the analytically tractable discrete-time Blanchard-Yaari perpetual-youth model with log utility and fair annuities: `blanchard_steady_state` bisects for the high-capital root, `blanchard_solve` linearizes the `(k, C)` saddle path, and `blanchard_transition` simulates convergence; government debt is net wealth so Ricardian equivalence fails.
`LifeCycleOLG` is the Auerbach-Kotlikoff / Imrohoroglu life-cycle alternative with age-specific mortality, an age-earnings profile, retirement onto pay-as-you-go pensions, and backward-induction EGM solved by `lifecycle_steady_state`; `lifecycle_transition` computes Auerbach-Kotlikoff shooting transitions after TFP or capital displacements.
`to_spec` wraps either family as a residual `ModelSpec` so `solve`/`irf` apply, and `blanchard_nk_spec` appends a small New Keynesian block to the perpetual-youth aggregates.

# Functions

| Function | Signature | Role |
|---|---|---|
| [BlanchardOLG](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `BlanchardOLG(; alpha=0.36, beta=0.96, delta=0.08, gamma=0.98, Z=1.0, b=0.0)` | Perpetual-youth calibration; `gamma = 1` is the Ramsey limit |
| [blanchard_steady_state](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `blanchard_steady_state(m::BlanchardOLG; tol=1e-10, max_iter=200)` | Bisection for the high-capital root; returns `BlanchardOLGSteadyState` |
| [blanchard_solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `blanchard_solve(m::BlanchardOLG, ss::BlanchardOLGSteadyState)` | Linearized `(k, C)` saddle path; returns `BlanchardOLGSolution` |
| [blanchard_transition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `blanchard_transition(m, sol, k0::Real; H=50)` | `(k, C, r, w)` convergence paths from initial capital `k0` |
| [to_spec](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `to_spec(m::BlanchardOLG; rho_z=0, sigma_z=0)` | Residual `ModelSpec` on `(k, C, r, w, Z)` for Gensys `solve`/`irf` |
| [blanchard_nk_spec](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/blanchard.jl) | `blanchard_nk_spec(m::BlanchardOLG; rho_z=0, sigma_z=0, kwargs...)` | Appends Phillips, Taylor, and Fisher residuals to the OLG aggregates |
| [LifeCycleOLG](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `LifeCycleOLG(; J=60, J_retire=45, survival=0.99, earnings, income, replacement=0.4, annuities=true, n_pop=0.0, beta=0.97, sigma=2.0, alpha=0.36, delta=0.06, Z=1.0, a_max=60.0, n_a=200, credit_limit=0.0, grid_type=:double_exp)` | Age-structured life-cycle calibration with retirement and pensions |
| [lifecycle_steady_state](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_steady_state(m::LifeCycleOLG; r_bounds=(-0.02, 0.10), tol=1e-6, max_iter=60, bequest_iter=50, verbose=false)` | Capital-market-clearing stationary equilibrium via bisection |
| [lifecycle_policies](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_policies(m::LifeCycleOLG, r, w; tau, pension)` | Backward age-EGM sweep at given prices; returns `(c_pol, a_pol)` |
| [lifecycle_distribution](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_distribution(m::LifeCycleOLG, a_pol; kwargs...)` | Forward cohort push to the unit-mass `n_a x n_e x J` distribution |
| [lifecycle_income](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_income(rho, sigma, n; method=:rouwenhorst)` | Idiosyncratic productivity in levels, normalized to unit mean |
| [lifecycle_survival](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_survival(J::Int; age0=21, makeham=0.0002, kwargs...)` | Gompertz-Makeham age-specific survival profile |
| [lifecycle_transition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_transition(m::LifeCycleOLG, k0::Real; H=80, tol=1e-5, max_iter=80, relax=0.5)` | Shooting transition from displaced capital; returns `LifeCycleTransition` |
| [lifecycle_transition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `lifecycle_transition(m::LifeCycleOLG, Z_path::AbstractVector; ss=nothing, tol=1e-5, max_iter=80, relax=0.5)` | Shooting transition along a TFP path returning to `Z` |
| [to_spec](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/olg/lifecycle.jl) | `to_spec(m::LifeCycleOLG; agent_name=:households)` | `LifeCycleSystem` wrapper; `solve` dispatches to `lifecycle_steady_state` |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/family_facades.jl) | `irf(ss::LifeCycleSteadyState, horizon; kwargs...)` | TFP-shock responses from the life-cycle transition machinery |
| [simulate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/family_facades.jl) | `simulate(ss::LifeCycleSteadyState, T_periods; kwargs...)` | Forward simulation from the life-cycle steady state |

# Examples

```julia
using MacroEconometricModels
m = BlanchardOLG(; alpha=0.36, beta=0.96, delta=0.08, gamma=0.98)
ss = blanchard_steady_state(m)
report(ss)
sol = blanchard_solve(m, ss)
```

# See also

* [DSGE Models](/dsge/dsge.md) - representative-agent pipeline; Gensys backend for `to_spec` wrappers
* [Continuous-Time Heterogeneous Agents](/dsge/ct.md) - infinite-horizon incomplete markets in continuous time
