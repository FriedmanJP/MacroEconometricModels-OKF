---
type: Feature
title: DSGE Models (Representative Agent, Nonlinear, Estimation, Heterogeneous Agents)
description: Full DSGE workflow from @dsge specification to linear and nonlinear solution, simulation, impulse responses, Bayesian and GMM estimation, and heterogeneous-agent extensions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/dsge
tags:
  - dsge
  - rational-expectations
  - gensys
  - perturbation
  - bayesian-estimation
  - heterogeneous-agents
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/dsge
  at: 2026-09-25T01:22:02Z
sources:
  - id: dsge-overview
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge.md
    title: DSGE Models overview page
  - id: dsge-linear
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_linear.md
    title: Linear Solution Methods page
  - id: dsge-nonlinear
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_nonlinear.md
    title: Nonlinear Solution Methods page
  - id: dsge-estimation
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_estimation.md
    title: DSGE Estimation page
  - id: dsge-constraints
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_constraints.md
    title: DSGE Constraints page
  - id: dsge-hd
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/dsge_hd.md
    title: DSGE Historical Decomposition page
  - id: src-dsge
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/dsge
    title: src/dsge module source
---

# Summary

The `dsge` module (44 files; the largest in the package by lines of code) covers the full representative-agent DSGE workflow: the `@dsge` macro parses equilibrium conditions into a `ModelSpec`, `compute_steady_state` locates the deterministic steady state, `linearize` produces the Sims (2002) canonical form, and `solve` dispatches to linear (`:gensys`, `:blanchard_kahn`, `:klein`), higher-order perturbation, global (`:projection`, `:pfi`, `:vfi`), perfect-foresight, and occasionally-binding-constraint (OccBin) solvers.
Downstream analysis — `simulate`, `irf`, `fevd`, `historical_decomposition`, GMM (`estimate_dsge`) and Bayesian (`estimate_dsge_bayes`) estimation — operates on the returned solution objects.
The `dsge/heterogeneous/` subdirectory adds incomplete-markets models (EGM/VFI household problems, Krusell-Smith, sequence-space Jacobian, Reiter, Winberry, DCEGM, Khan-Thomas, Bewley Banks) that dispatch through the same `solve`/`compute_steady_state` interface on the agent kind.

# Functions

Table capped at the 50 most important exported entry points; see the truncation note below.

| Function | Signature | Role |
|---|---|---|
| [@dsge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/parser.jl) | `@dsge begin ... end` | Specification DSL; parses equations into `ModelSpec` |
| [compute_steady_state](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/steady_state.jl) | `compute_steady_state(spec; initial_guess=nothing, method=:auto, algorithm, constraints=nothing, solver=:auto)` | Numerical (NonlinearSolve) or analytical steady state |
| [linearize](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/linearize.jl) | `linearize(spec::ModelSpec)` | First-order Taylor expansion; returns Sims canonical form `LinearDSGE` |
| [solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/gensys.jl) | `solve(spec::ModelSpec; method=:gensys, kwargs...)` | Single entry point; dispatches on `method` and agent kind |
| [gensys](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/gensys.jl) | `solve(spec; method=:gensys, div=1.0+1e-8, rank_rtol=1e-8, sparse=:auto)` | Default linear solver; undetermined-coefficients iteration with companion-QZ fallback |
| [blanchard_kahn](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/blanchard_kahn.jl) | `solve(spec; method=:blanchard_kahn, div, cluster_tol, sparse)` | Linear solver via companion-QZ solvent |
| [klein](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/klein.jl) | `solve(spec; method=:klein, div, cluster_tol, sparse)` | Linear solver via companion-QZ solvent |
| [determinacy_region](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/determinacy.jl) | `determinacy_region(spec; params, grids, div, rank_rtol, method=:gensys, threaded=false)` | Sweep 1-2 parameters; returns `DeterminacyMap` of verdicts |
| [determinacy_boundary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/determinacy.jl) | `determinacy_boundary(m::DeterminacyMap)` | Locate determinacy frontier within half a grid step |
| [is_determined](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/types.jl) | `is_determined(sol)` | True iff `eu == [1, 1]` (Sims rank conditions) |
| [is_stable](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/types.jl) | `is_stable(sol)` | True iff max eigenvalue modulus of `G1` is below 1 |
| [nshocks](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/types.jl) | `nshocks(sol)` | Number of exogenous shocks (`spec.n_exog`) |
| [perturbation_solver](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/perturbation.jl) | `perturbation_solver(spec; order=2, sylvester_method=:auto, sylvester_tol=1e-8)` | Orders 1-3 perturbation (SGU 2004; Andreasen et al. 2018) |
| [pruned_state_space](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/pruned_state_space.jl) | `pruned_state_space(sol::PerturbationSolution)` | Pruned system object (Kim et al. 2008) backing simulation and moments |
| [collocation_solver](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/projection.jl) | `collocation_solver(spec; degree=5, grid=:auto, smolyak_mu=3, quadrature=:auto, tol=1e-8, max_iter=100, threaded=false)` | Global Chebyshev collocation (tensor or Smolyak grids) |
| [pfi_solver](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/pfi.jl) | `pfi_solver(spec; kwargs...)` | Global Euler-equation time iteration (Coleman 1990); no reward needed |
| [vfi_solver](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/vfi.jl) | `vfi_solver(spec; utility, beta, consumption, controls, transition, control_bounds, outcome, degree, n_grid, howard_steps)` | Bellman value-function iteration with Howard policy evaluation |
| [evaluate_policy](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/projection.jl) | `evaluate_policy(sol::ProjectionSolution, x_state)` | Evaluate global policy at a state vector (or matrix of states) |
| [max_euler_error](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/projection.jl) | `max_euler_error(sol::ProjectionSolution; n_test=1000, rng)` | Max Euler-equation error on random test points |
| [perfect_foresight](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/perfect_foresight.jl) | `perfect_foresight(spec; kwargs...)` | Newton solver for deterministic transition paths |
| [variable_bound](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/constraints.jl) | `variable_bound(var::Symbol; lower=nothing, upper=nothing)` | Box constraint constructor (e.g. ZLB) |
| [nonlinear_constraint](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/constraints.jl) | `nonlinear_constraint(fn::Function; label="constraint")` | Nonlinear inequality constraint `fn(...) <= 0` |
| [parse_constraint](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/occbin.jl) | `parse_constraint(expr::Expr, spec::ModelSpec)` | Parse `var[t] >= bound` into `OccBinConstraint` |
| [occbin_solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/occbin.jl) | `occbin_solve(spec, constraint; kwargs...)` | Guess-and-verify piecewise-linear solution (Guerrieri-Iacoviello 2015) |
| [occbin_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/occbin.jl) | `occbin_irf(spec, constraint, shock_idx::Int, horizon::Int; magnitude=1.0, maxiter=100)` | IRFs under occasionally binding constraints (two-constraint method also available) |
| [simulate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/simulation.jl) | `simulate(sol, T_periods; shock_draws=nothing, rng)` | Stochastic forward simulation in levels (pruned for perturbation) |
| [irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/simulation.jl) | `irf(sol, horizon; irf_type=:analytical, n_draws=500, shock_size=1.0)` | Analytical IRFs, or GIRFs via Monte Carlo for nonlinear solutions |
| [fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/simulation.jl) | `fevd(sol, horizon; unconditional=false)` | h-step FEVD, or unconditional (augmented Lyapunov) at order >= 2 |
| [solve_lyapunov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/analytical.jl) | `solve_lyapunov(G1, impact)` | Unconditional covariance via doubling iteration + Bartels-Stewart fallback |
| [analytical_moments](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/analytical.jl) | `analytical_moments(sol; lags=1)` | Moment vector matching `autocovariance_moments` for GMM |
| [historical_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/hd.jl) | `historical_decomposition(sol, data, observables; measurement_error)` | Kalman-smoother shock attribution; Bayesian variant adds credible bands |
| [dsge_smoother](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/smoother.jl) | `dsge_smoother(ss::DSGEStateSpace, data)` | Rauch-Tung-Striebel smoother for the linear state space |
| [estimate_dsge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/estimation.jl) | `estimate_dsge(spec, data, param_names; method=:irf_matching, ...)` | GMM estimation: `:irf_matching`, `:euler_gmm`, `:smm`, `:analytical_gmm` |
| [estimate_dsge_bayes](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `estimate_dsge_bayes(spec, data, theta0; priors, method=:smc, observables, n_smc)` | Bayesian estimation via `:smc`, `:smc2`, or RWMH |
| [posterior_mode](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `posterior_mode(spec, data, theta0; priors, observables)` | Posterior mode + Laplace-approximation marginal likelihood |
| [posterior_summary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `posterior_summary(result::BayesianDSGE; min_ess=400)` | Posterior means, credible intervals, ESS per parameter |
| [marginal_likelihood](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `marginal_likelihood(result::BayesianDSGE)` | Log marginal likelihood from the sampler |
| [bayes_factor](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `bayes_factor(r1::BayesianDSGE, r2::BayesianDSGE)` | Log Bayes factor comparing two fitted models |
| [mcmc_diagnostics](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/mcmc_diagnostics.jl) | `mcmc_diagnostics(result::BayesianDSGE)` | R-hat, ESS, and convergence diagnostics |
| [bridge_sampling_ml](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/bayes_estimation.jl) | `bridge_sampling_ml(result::BayesianDSGE; kwargs...)` | Bridge-sampling marginal likelihood estimate |
| [identification_diagnostics](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/identification.jl) | `identification_diagnostics(spec, param_names; kwargs...)` | Local identification checks for estimated parameters |
| [dynare_prior](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/priors.jl) | `dynare_prior(dist::Symbol, mean_v, std_v; kwargs...)` | Dynare-compatible prior distributions incl. `InverseGamma1` |
| [apply_prefilter](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/prefilter.jl) | `apply_prefilter(data, transform::Symbol; kwargs...)` | Detrending/prefiltering of observables (`PrefilterSpec`, `invert_prefilter`) |
| [load_ha_example](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/examples.jl) | `load_ha_example(name::Symbol; distribution=:young)` | Load HA example economies (`:krusell_smith`, one-/two-asset HANK, ...) |
| [rouwenhorst](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/types.jl) | `rouwenhorst(rho, sigma, n; sigma_is=:innovation)` | Rouwenhorst AR(1) discretization (log grid, `IncomeProcess`) |
| [tauchen](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/types.jl) | `tauchen(rho, sigma, n; m=3, sigma_is=:innovation)` | Tauchen AR(1) discretization (log grid, `IncomeProcess`) |
| [ssj_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/blocks.jl) | `ssj_irf(gej::SSJGEJacobian, dZ::AbstractDict; kwargs...)` | Sequence-space IRFs from the GE Jacobian (Auclert et al. 2021) |
| [den_haan_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/krusell_smith.jl) | `den_haan_test(sol; kwargs...)` | Den Haan accuracy test for Krusell-Smith / SSJ / Reiter dynamics |
| [distribution_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/analysis.jl) | `distribution_irf(sol::HADSGESolution, horizon; kwargs...)` | IRFs of the wealth distribution itself (Reiter method) |
| [dcegm_solve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dsge/heterogeneous/dcegm.jl) | `dcegm_solve(prob::DCEGMProblem; max_iter=500, tol=1e-8)` | Discrete-continuous choice solver (Iskhakov et al. 2017) |

Truncation note: omitted HA entry points available in `dsge/heterogeneous/`: `solve(spec; method=:ssj/:reiter/:krusell_smith)` aggregate solvers, `combine_blocks`/`block_jacobian` SSJ composition, `fit_winberry`/`winberry_moments` parametric distributions, `khan_thomas_steady_state`/`khan_thomas_mit` lumpy investment, `intermediary_steady_state`/`intermediary_mit` Bewley Banks, `dcegm_steady_state`/`dcegm_mit`/`dcegm_retirement_model`, `inequality_irf`/`simulate_panel`, `ha_grid_diagnostics`/`adaptive_asset_grid`, plus `dsge_particle_smoother`, `posterior_predictive`, `prior_predictive`, `learning_rate_check`, and `to_spec` family wrappers.

# Examples

```julia
using MacroEconometricModels
spec = @dsge begin
    parameters: β = 0.99, α = 0.36, δ = 0.025, ρ = 0.9, σ = 0.01
    endogenous: Y, C, K, A
    exogenous: ε_A
    Y[t] = A[t] * K[t-1]^α
    C[t] + K[t] = Y[t] + (1 - δ) * K[t-1]
    1 = β * (C[t] / C[t+1]) * (α * A[t+1] * K[t]^(α - 1) + 1 - δ)
    A[t] = A[t-1]^ρ * exp(σ * ε_A[t])
    steady_state = begin
        A_ss = 1.0
        K_ss = (α * β / (1 - β * (1 - δ)))^(1 / (1 - α))
        Y_ss = K_ss^α
        C_ss = Y_ss - δ * K_ss
        [Y_ss, C_ss, K_ss, A_ss]
    end
end
spec = compute_steady_state(spec)
sol = solve(spec)
result = irf(sol, 40)
```

# See also

* [Continuous-Time Heterogeneous Agents](ct.md) - HJB/KFE finite-difference Aiyagari and two-asset HANK
* [Overlapping Generations](olg.md) - Blanchard perpetual youth and life-cycle OLG
* [State-Space Models (Unobserved Components, TVP)](../nonlinear-statespace/statespace.md) - Kalman filtering and RTS smoothing machinery underlying the DSGE likelihood and smoother
* [Innovation Accounting](../multivariate/var.md) - IRF/FEVD/HD layer for VAR-family models
