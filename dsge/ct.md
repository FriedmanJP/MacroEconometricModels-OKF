---
type: Feature
title: Continuous-Time Heterogeneous Agents
description: Achdou et al. (2022) finite-difference HJB and Kolmogorov-Forward solvers for one-asset Aiyagari and two-asset Kaplan-Moll-Violante economies with MIT-shock transitions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/ct
tags:
  - dsge
  - continuous-time
  - heterogeneous-agents
  - hjb
  - hank
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/dsge
  at: 2026-09-25T01:22:02Z
sources:
  - id: dsge-continuous
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_continuous.md
    title: Continuous Time page
  - id: dsge-heterogeneity
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_heterogeneity.md
    title: Heterogeneity and Continuous Time sub-hub page
  - id: src-ct
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/ct
    title: src/ct module source
---

# Summary

The `ct` module solves continuous-time incomplete-markets economies with the sparse finite-difference methods of Achdou et al. (2022): the household problem is a Hamilton-Jacobi-Bellman PDE solved by an implicit upwind scheme (`ct_hjb`), and the stationary wealth distribution solves the Kolmogorov-Forward equation reusing the same infinitesimal generator (`ct_kfe`).
`ct_steady_state` bisects on the interest rate to clear the capital market; `ct_mit_shock` computes deterministic transitions by shooting on the capital path.
The two-asset extension (`CTTwoAsset`) adds liquid/illiquid portfolio choice with convex deposit costs in the Kaplan-Moll-Violante (2018) tradition, closed in general equilibrium by `ct_two_asset_ge` and transitioned by `ct_two_asset_mit`.
`solve(to_spec(m))` dispatches to the matching steady-state solver, and `irf(m, horizon; ss, shock_size)` wraps the MIT shock as an `ImpulseResponse`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [CTAiyagari](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/continuous_aiyagari.jl) | `CTAiyagari(; alpha=0.36, rho=0.05, sigma=2.0, delta=0.05, z, lambda, a_max, I)` | One-asset Aiyagari calibration with two-state Poisson income |
| [ct_hjb](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/continuous_aiyagari.jl) | `ct_hjb(m::CTAiyagari, r, w; max_iter=100, tol=1e-6, Delta=1000.0)` | Implicit upwind HJB solve; returns `(v, c, s, A, a, converged)` |
| [ct_kfe](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/continuous_aiyagari.jl) | `ct_kfe(A::SparseMatrixCSC, I::Int, da)` | Stationary density from `A'g = 0`, normalized to one |
| [ct_steady_state](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/continuous_aiyagari.jl) | `ct_steady_state(m::CTAiyagari; r_bounds=(0.0001, rho-1e-4), max_iter=100, tol=1e-6, hjb_max_iter=100, Delta=1000.0)` | Interest-rate bisection to a `CTSteadyState` |
| [ct_mit_shock](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/continuous_aiyagari.jl) | `ct_mit_shock(m, ss0::CTSteadyState, Z_path; dt=0.25, max_iter=300, tol=1e-6, relax=0.3)` | MIT-shock transition via capital-path shooting; returns `CTTransition` |
| [CTTwoAsset](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `CTTwoAsset(; sigma=2.0, rho=0.06, r_a=0.05, r_b=0.02, chi=2.0, cost=:quadratic, a_max, b_max, Ib, Ia, ...)` | Two-asset KMV calibration: liquid/illiquid returns, deposit cost, grids |
| [ct_two_asset_solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `ct_two_asset_solve(m::CTTwoAsset; max_iter=200, tol=1e-6, Delta=1000.0, check_stationarity=true, V_init=nothing)` | Two-dimensional HJB + KFE; returns `CTTwoAssetSolution` |
| [ct_two_asset_ge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `ct_two_asset_ge(m::CTTwoAsset; K_init, rb_init, max_iter=60, tol=1e-4, relax_K=0.3, relax_rb=0.02, hjb_max_iter=200)` | General equilibrium: clears illiquid capital and liquid bond markets |
| [ct_two_asset_mit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `ct_two_asset_mit(m, ge0::CTTwoAssetGE, Z_path; dt=0.25, max_iter=200, tol=1e-5, relax_K=0.3, relax_rb=0.02)` | Two-asset MIT transition shooting on capital and liquid-return paths |
| [hand_to_mouth](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `hand_to_mouth(s::CTTwoAssetSolution; b_threshold, a_threshold)` | Poor vs wealthy hand-to-mouth population shares |
| [ceiling_mass](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `ceiling_mass(s::CTTwoAssetSolution)` | Stationary mass on the top grid nodes (truncation diagnostic) |
| [ct_two_asset_stationarity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/ct/two_asset.jl) | `ct_two_asset_stationarity(m::CTTwoAsset; margin=0.9, solution=nothing, max_ceiling_mass=nothing)` | `a_max <= a*` bound check; returns `(ok, bound, a_star, message, ceiling_mass)` |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/family_facades.jl) | `irf(m::Union{CTAiyagari,CTTwoAsset}, horizon; ss, shock_size=0.01)` | MIT shock wrapped as an `ImpulseResponse` for `plot_result` |
| [simulate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/family_facades.jl) | `simulate(m::Union{CTAiyagari,CTTwoAsset}, T_periods; kwargs...)` | Forward simulation from the stationary equilibrium |

# Examples

```julia
using MacroEconometricModels
m = CTAiyagari(; alpha=0.36, rho=0.05, sigma=2.0, delta=0.05,
                 z=[0.1, 0.2], lambda=[0.5, 0.5], a_max=30.0, I=200)
ss = ct_steady_state(m; tol=1e-5)
report(ss)
```

# See also

* [DSGE Models](dsge.md) - representative-agent pipeline and discrete-time heterogeneous agents
* [Overlapping Generations](olg.md) - finite-horizon demographics and life-cycle saving
