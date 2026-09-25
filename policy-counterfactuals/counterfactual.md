---
type: Feature
title: Policy Counterfactuals
description: Sufficient-statistics policy counterfactuals via McKay-Wolf rule projections, Barnichon-Mesters optimal perturbations, and Caravello-McKay-Wolf model-bank discipline with honesty diagnostics.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/counterfactual
tags:
  - counterfactual
  - policy
  - monetary-policy
  - opp
  - irf-matching
  - dsge
status: approved
reviewed_by: chung9207
reviewed_at: 2026-09-25
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/small-domains
  at: 2026-09-25T01:25:44Z
sources:
  - id: counterfactual-hub
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/counterfactual.md
    title: Policy Counterfactuals docs hub
  - id: counterfactual-rules
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/counterfactual_rules.md
    title: Rule Counterfactuals (McKay-Wolf) docs
  - id: counterfactual-opp
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/counterfactual_opp.md
    title: Optimal Policy Perturbations (Barnichon-Mesters) docs
  - id: counterfactual-bank
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/counterfactual_bank.md
    title: Model Bank and Diagnostics (Caravello-McKay-Wolf) docs
  - id: src-counterfactual
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/counterfactual
    title: src/counterfactual module source
---

# Summary

The `counterfactual` module answers "what would the economy have looked like under a different policy?" with the sufficient-statistics approach of McKay-Wolf (2023), Barnichon-Mesters (2023), and Caravello-McKay-Wolf (2025).
All three strands reduce to one weighted projection over a policy-shock vector, with the load-bearing input a `PolicyCausalEffects` container whose columns are impulse responses to identified policy shocks.
McKay-Wolf builds Lucas-robust rule counterfactuals and loss-minimizing optimal policy from a baseline plus a news menu; Barnichon-Mesters tests announced-policy optimality with the OPP statistic and constrained recommendations; Caravello-McKay-Wolf disciplines model-implied menus with limited-information IRF matching, model averaging, and historical counterfactuals.
Honesty diagnostics (`rel_residual`, `spanning_diagnostic`, `forecast_sufficiency`) quantify enforceability wherever identified shocks fall short of the desired policy change.

# Functions

| Function | Signature | Role |
|---|---|---|
| [rate_peg_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `rate_peg_rule(H::Int; outcomes=[:infl, :ygap], instruments=[:rate])` | Counterfactual instrument-peg rule template (`z = 0`) |
| [rate_target_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `rate_target_rule(H::Int, path; outcomes, instruments)` | Arbitrary instrument-path rule template |
| [inflation_target_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `inflation_target_rule(H::Int; pi_var=:infl, outcomes, instruments)` | Strict inflation-targeting rule template |
| [output_gap_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `output_gap_rule(H::Int; y_var=:ygap, outcomes, instruments)` | Strict output-gap-targeting rule template |
| [ngdp_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `ngdp_rule(H::Int; pi_var=:infl, y_var=:ygap, outcomes, instruments)` | NGDP-level-targeting rule template |
| [taylor_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `taylor_rule(H::Int; rho=0.5, phi_pi=1.5, phi_y=1.0, z_lag=0.0, ...)` | Inertial Taylor-rule template |
| [custom_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/rules.jl) | `custom_rule(A_x, A_z; outcomes, instruments, wedge=nothing, name="custom")` | Raw-matrix validated escape hatch for user rules |
| [weight_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/loss.jl) | `weight_matrix(H::Int; lambda=1.0, beta=1.0)` | Discounted diagonal loss weight matrix |
| [policy_loss](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/loss.jl) | `policy_loss(outcomes, H::Int; lambda, beta=1.0, instruments, W_z=nothing, ...)` | Quadratic `PolicyLoss` builder with discounted diagonal blocks |
| [ait_loss](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/loss.jl) | `ait_loss(H::Int; beta, lambda_avg=0.6, lambda_t=0.4, lambda_y=1.0, ...)` | Average-inflation-targeting loss (PSD-singular by construction) |
| [smoothing_penalty](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/loss.jl) | `smoothing_penalty(H::Int; lambda=1.0, beta=1.0, z_lag=0.0)` | Instrument-smoothing penalty with initial-condition linear term |
| [policy_causal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/empirical.jl) | `policy_causal_effects(bir::BayesianImpulseResponse, shocks, outcomes, instruments; H, normalize, source)` | Causal-effects container from BVAR posterior IRFs |
| [policy_causal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/empirical.jl) | `policy_causal_effects(slp::StructuralLP, shocks, outcomes, instruments; H, n_draws, ...)` | Causal-effects container from structural local projections |
| [policy_causal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/empirical.jl) | `policy_causal_effects(lpr::LPImpulseResponse, outcomes, instruments; H, n_draws, ...)` | Causal-effects container from LP impulse responses |
| [policy_causal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_ha.jl) | `policy_causal_effects(spec::ModelSpec, ss; outcomes, instruments, H, T_horizon, rule_closure, ...)` | Rate-wedge news menus from heterogeneous-agent sequence-space Jacobians |
| [baseline_path](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/empirical.jl) | `baseline_path(ir, nonpolicy_shock, outcomes, instruments; H, negate=false)` | Baseline response path to a non-policy disturbance |
| [wold_representation](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/empirical.jl) | `wold_representation(m; H, orthogonalize=:cholesky, ...)` | Wold MA representation from VAR/BVAR fits for second-moment counterfactuals |
| [policy_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/forecast.jl) | `policy_forecast(fc, outcomes; targets, H, origin)` | Gap-typed `PolicyForecast` from package VAR/BVAR forecasts |
| [policy_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/forecast.jl) | `policy_forecast(outcomes, values; sd=nothing, rho=0.9, n_draws=1000, ...)` | Gap-typed `PolicyForecast` from external point plus dispersion inputs |
| [interp_to_quarterly](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/forecast.jl) | `interp_to_quarterly(annual, H::Int)` | Interpolate annual SEP-style points to a quarterly path |
| [stacked_irf_target](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/irf_target.jl) | `stacked_irf_target(ce; order=:shock_major, scale, drop, inflate)` | Stacked empirical IRF target for limited-information matching |
| [ctw_covariance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/irf_target.jl) | `ctw_covariance(V_bar, block_len::Int; bandwidth=8, eta=1.0)` | CTW-damped covariance for the IRF-matching quasi-likelihood |
| [policy_news_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_dsge.jl) | `policy_news_matrix(spec, policy_shock, outcomes, instruments; H=100, solver=:gensys, chunk=0)` | Square news menu from a linear DSGE via an augmented news pipeline |
| [sequence_jacobian](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_ha.jl) | `sequence_jacobian(spec, ss, input, output; T_horizon=300, dx=1e-4)` | Fake-news sequence-space Jacobians with anticipation |
| [cognitive_discounting](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/behavioral.jl) | `cognitive_discounting(J::AbstractMatrix, m::Real)` | Gabaix cognitive discounting of a sequence-space Jacobian |
| [sticky_expectations](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/behavioral.jl) | `sticky_expectations(J::AbstractMatrix, theta::Real)` | Sticky-expectations operator on a sequence-space Jacobian |
| [behavioral](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/behavioral.jl) | `behavioral(ce::PolicyCausalEffects; m=1.0, theta=0.0)` | Behavioral variant of a whole square container |
| [policy_counterfactual](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/counterfactual.jl) | `policy_counterfactual(base, ce, rule; method=:auto, draws=:auto, ...)` | Lucas-robust paths under an alternative rule |
| [optimal_policy](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/optimal_policy.jl) | `optimal_policy(base, ce, loss; z_wedge=nothing, draws=:auto, ...)` | Loss-minimizing projection over the reachable set |
| [optimal_rule](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/optimal_policy.jl) | `optimal_rule(ce, loss; z_wedge=nothing)` | Implied optimal targeting rule closing the optimal-policy circle |
| [counterfactual_moments](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/moments.jl) | `counterfactual_moments(wold, ce, policy; outcomes, instruments, draws=:auto, ...)` | Second-moment counterfactuals under invertibility |
| [opp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/opp.jl) | `opp(fc, ce, loss; instrument_path=nothing, z_wedge=nothing)` | Optimal policy perturbation point statistic and optimality test |
| [estimate_opp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/opp.jl) | `estimate_opp(fc, ce, loss; independent=true, levels=(0.60, 0.75, 0.90), n_sim=2000, ...)` | Two-source OPP inference with 60/75/90% bands |
| [constrained_opp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/constrained.jl) | `constrained_opp(fc, ce, loss, constraints; instrument_path, method=:auto, ...)` | SLSQP constrained OPP for ZLB and pre-commitment pledges |
| [zlb_constraint](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/constrained.jl) | `zlb_constraint(; floor=0.0, instrument=:rate, horizons=1:typemax(Int))` | Zero-lower-bound linear path-floor constraint |
| [opp_sequence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/opp_sequence.jl) | `opp_sequence(forecasts, ce, loss; dates=nothing, ce_by_date=nothing, ...)` | Decision-date OPP history |
| [opp_sensitivity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/opp_sequence.jl) | `opp_sensitivity(forecasts, ce, H::Int; lambda_grid, build_loss, ...)` | OPP sequences over a loss-weight grid |
| [robust_weights](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/opp_sequence.jl) | `robust_weights(seq_builder, theta_grid)` | Time-consistent robust loss weights across sequences |
| [irf_match](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_bank.jl) | `irf_match(menu_builder, target, priors, param_names; name, H_news=25, ...)` | Limited-information CMW quasi-likelihood estimation of one bank member |
| [posterior_model_probs](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_bank.jl) | `posterior_model_probs(members; prior)` | Posterior model probabilities over bank members |
| [model_average](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/model_bank.jl) | `model_average(members, probs; n_pool=1000, subset=nothing, ...)` | Pooled news menu stacking sampled menus with model uncertainty |
| [counterfactual_forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/historical.jl) | `counterfactual_forecast(m, data_through_tstar, ce, policy; outcomes, instruments, H, ...)` | Conditional counterfactual forecast from a break date |
| [counterfactual_history](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/historical.jl) | `counterfactual_history(m, data, t_range, ce, policy; outcomes, instruments, ...)` | Historical counterfactual via forecast revisions |
| [spanning_diagnostic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/diagnostics.jl) | `spanning_diagnostic(base, ce_emp, ce_full, policy; draws=:auto, tol=0.1, ...)` | Share of the desired policy change outside the identified span |
| [forecast_sufficiency](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/diagnostics.jl) | `forecast_sufficiency(sol::DSGESolution, observables; H=40)` | Model-laboratory measure of forecast-sufficiency for historical counterfactuals |
| [is_square](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/counterfactual/types.jl) | `is_square(ce::PolicyCausalEffects)` | Whether a container is a square model menu or a thin empirical one |

# Examples

```julia
using MacroEconometricModels
H = 20
ce = policy_news_matrix(spec, :eps_i, [:infl => :π, :ygap => :y], [:rate => :i]; H=H)
base = baseline_path(irf(solve(spec), H), "eps_d",
                     [:infl => "π", :ygap => "y"], [:rate => "i"]; H=H)
pc = policy_counterfactual(base, ce, rate_peg_rule(H))
report(pc)
```

# See also

* [Vector Autoregression (VAR)](/multivariate/var.md) - reduced-form IRFs feeding empirical causal-effect containers
* [Bayesian VAR (BVAR)](/multivariate/bvar.md) - posterior IRF draws for uncertainty bands
* [Local Projections (LP)](/multivariate/lp.md) - direct IRF estimates as policy menus
* [DSGE](/dsge/dsge.md) - linear model news menus and laboratory validation
* [Input-Output Analysis (IO)](/io/io.md) - sectoral structure for applied policy analysis
