---
type: Feature
title: Simulation (Data-Generating Processes)
description: NamedTuple-returning simulators with explicit RNGs, burn-in, and population truth for Monte Carlo and recovery checks.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/dgp
tags:
  - infrastructure
  - simulation
  - dgp
  - monte-carlo
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/infrastructure
  at: 2026-09-25T01:38:10Z
sources:
  - id: simulation-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/simulation.md
    title: Simulation (DGPs) docs page
  - id: src-dgp
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/dgp
    title: src/dgp module source
---

# Summary

The `dgp` module ships public data-generating processes for Monte Carlo experiments and estimator recovery checks. Every simulator takes an explicit `rng` first (never touching the global RNG), discards a burn-in for dynamic designs, and returns a NamedTuple pairing the simulated sample with population truth: coefficients, covariances, shocks, and latent paths. Coverage spans VARs (with population IRF/FEVD/HD helpers), non-Gaussian and heteroskedastic SVARs, univariate designs, cointegration/ARDL/panel VAR, the GARCH family plus SV/MGARCH/MIDAS, factor and mixed-frequency panels, LP/treatment designs, cross-section/panel/staggered-DiD, regime switching, GMM/policy-band/DSGE-measurement designs, and deterministic analytic-truth helpers.

# Functions

| Function | Signature | Role |
|---|---|---|
| [dgp_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_var.jl) | `dgp_var(rng::AbstractRNG; A, B0=nothing, Sigma=nothing, c=nothing, T=500, burn=200)` | Stationary VAR(p); returns `(Y, eps, A, Sigma, B0, c)` |
| [lyapunov_gamma0](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_var.jl) | `lyapunov_gamma0(A, Sigma)` | Stationary covariance `Γ_0` of a VAR(1) |
| [var_irf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_var.jl) | `var_irf(A, B0, H::Int)` | Population `(H+1)×n×n` structural moving average |
| [var_fevd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_var.jl) | `var_fevd(A, B0, H::Int)` | Population variance-decomposition shares |
| [var_hd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_var.jl) | `var_hd(A, B0, eps; c=nothing)` | Population `T×n×n` historical shock contributions |
| [dgp_nongaussian_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_svar_nongauss.jl) | `dgp_nongaussian_var(rng::AbstractRNG; A, B0, dist=:t, nu=5.0, T=1000, burn=200)` | VAR with independent non-Gaussian shocks |
| [dgp_heteroskedastic_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_svar_nongauss.jl) | `dgp_heteroskedastic_var(rng::AbstractRNG; kind=:markov, Lambda, T=1000, burn=200, ...)` | VAR with `:markov`/`:garch`/`:smooth`/`:external` variance path (anything else throws) |
| [dgp_arima](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_arima(rng::AbstractRNG; phi, theta, d=0, Phi, Theta, s=0, c=0.0, sigma=1.0, T=500, burn=200)` | Seasonal ARIMA with Gaussian innovations |
| [dgp_trend_cycle](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_trend_cycle(rng::AbstractRNG; trend=:rw, drift=0.1, period=16.0, rho=0.9, ...)` | Random-walk/linear trend plus AR(2) cycle |
| [dgp_ar2_peak](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_ar2_peak(rng::AbstractRNG; period=8.0, modulus=0.9, sigma=1.0, T=1000, burn=200)` | AR(2) with analytic spectrum on a 256 grid |
| [dgp_lagged_pair](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_lagged_pair(rng::AbstractRNG; d=3, gain=2.0, T=1000, burn=200)` | Lagged pair with known phase and gain |
| [dgp_state_space](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_state_space(rng::AbstractRNG; F, H, Q, R, b=nothing, d=nothing, ...)` | Linear Gaussian state space with true state path |
| [dgp_unit_root_pair](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_univariate.jl) | `dgp_unit_root_pair(rng::AbstractRNG; kind=:adf, T=200, ...)` | Null/alternative pairs for thirteen test families |
| [dgp_vecm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_vecm(rng::AbstractRNG; alpha, beta, Gamma, mu=nothing, Sigma=nothing, T=400, burn=200)` | VECM with rank, adjustment, and short-run truth |
| [dgp_cointreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_cointreg(rng::AbstractRNG; beta, T=500, endog_rho=0.7, sigma_u=1.0, spurious=false)` | Cointegrating regression with endogenous errors |
| [dgp_panel_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_panel_var(rng::AbstractRNG; A1, N=30, T=25, mu_sd=1.0, Sigma=nothing, burn=50)` | Panel VAR(1) with random effects, stacked output |
| [dgp_ardl](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_ardl(rng::AbstractRNG; phi=0.6, beta0=0.8, beta1=0.4, rho_x=0.7, c=0.5, T=300, burn=100)` | ARDL(1,1) with long-run multiplier truth |
| [dgp_nardl](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_nardl(rng::AbstractRNG; phi=0.6, beta_pos=0.9, beta_neg=0.3, ...)` | NARDL with partial sums and asymmetric multipliers |
| [dgp_pmg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_cointegration.jl) | `dgp_pmg(rng::AbstractRNG; theta=1.5, N=20, T=50, homogeneous=true, burn=50)` | Panel ARDL with common or heterogeneous long run |
| [dgp_garch_family](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_volatility.jl) | `dgp_garch_family(rng::AbstractRNG; kind=:garch, omega=0.02, alpha=0.08, beta=0.88, innov=:gauss, T=3000, burn=500, ...)` | Nine-kind GARCH family; returns `(y, h, eps)` |
| [dgp_sv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_volatility.jl) | `dgp_sv(rng::AbstractRNG; mu=-0.5, phi=0.95, sigma_eta=0.2, rho_lev=0.0, nu=Inf, T=1500, burn=200)` | Stochastic volatility with leverage and t shocks |
| [dgp_mgarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_volatility.jl) | `dgp_mgarch(rng::AbstractRNG; kind=:ccc, n=2, T=1000, R, a=0.05, b=0.9, ...)` | CCC/DCC/BEKK with true covariance path |
| [dgp_midas](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_volatility.jl) | `dgp_midas(rng::AbstractRNG; m=3, K=12, T_lf=200, theta, kind=:expalmon, beta=1.0, ...)` | MIDAS with normalized true weights |
| [dgp_dynamic_factors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_factors.jl) | `dgp_dynamic_factors(rng::AbstractRNG; A, Lambda=nothing, r=2, p=1, N=40, T=400, ...)` | Dynamic factor model with VAR(p) factors |
| [dgp_mixed_frequency_panel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_factors.jl) | `dgp_mixed_frequency_panel(rng::AbstractRNG; A, Lambda_M=nothing, Lambda_Q=nothing, r=2, nM=10, nQ=2, T=240, ...)` | Monthly plus quarterly Mariano-Murasawa ragged panel |
| [dgp_lp_iv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_lp.jl) | `dgp_lp_iv(rng::AbstractRNG; T=400, pi1=1.5, theta=1.0)` | LP-IV with instrument, endogenous shock, outcome |
| [dgp_state_dependent_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_lp.jl) | `dgp_state_dependent_var(rng::AbstractRNG; A_exp, A_rec, B0, gamma=3.0, T=2000, burn=200, H=12)` | Two-regime VAR with logistic transition and regime IRFs |
| [dgp_propensity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_lp.jl) | `dgp_propensity(rng::AbstractRNG; beta_ps, tau=1.0, gamma_y, confounding=true, n=2000)` | Confounded selection with known ATT |
| [dgp_hac](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_lp.jl) | `dgp_hac(rng::AbstractRNG; rho=0.5, T=2000, k=1, x_first=false)` | AR(1)-error regression with population long-run variance |
| [dgp_cross_section](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_micro.jl) | `dgp_cross_section(rng::AbstractRNG; kind=:ols, beta, n=1000, ...)` | Fifteen-kind cross-section with kind-specific truth |
| [dgp_panel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_micro.jl) | `dgp_panel(rng::AbstractRNG; N=200, T=20, beta, sigma_u=1.0, sigma_e=1.0, ...)` | Linear/logit/probit panel with Mundlak effects |
| [dgp_staggered_did](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_micro.jl) | `dgp_staggered_did(rng::AbstractRNG; cohorts, tau, never_treated_share=0.3, N=300, T=25, ...)` | Staggered adoption with heterogeneous effects |
| [dgp_regime_switching](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_regime.jl) | `dgp_regime_switching(rng::AbstractRNG; kind=:ms, T=600, burn=100, ...)` | Markov-switching, SETAR, LSTAR, and ESTR designs |
| [dgp_gmm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_gmm.jl) | `dgp_gmm(rng::AbstractRNG; kind=:iv, beta, n=1000, hetero=true, overid_k=2, invalid_k=0, pi1=1.0)` | Heteroskedastic GMM with optional invalid instruments |
| [dgp_pce_draws](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_gmm.jl) | `dgp_pce_draws(rng::AbstractRNG, ce_point; sd=0.1, corr=0.0, n_draws=500)` | Policy-menu draws with known noise scale |
| [dgp_dsge_observed](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_gmm.jl) | `dgp_dsge_observed(rng::AbstractRNG, y_clean; H=nothing, trends=nothing)` | DSGE observation equation with measurement error |
| [arma_spectrum](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_truth.jl) | `arma_spectrum(phi, theta, sigma, freqs)` | Closed-form ARMA spectral density |
| [mm_aggregate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_truth.jl) | `mm_aggregate(F, Lambda_Q; weights=[1, 2, 3, 2, 1])` | Mariano-Murasawa quarterly aggregation |
| [logit_ame](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_truth.jl) | `logit_ame(X, beta)` | Closed-form logit average marginal effects |
| [probit_ame](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/dgp/dgp_truth.jl) | `probit_ame(X, beta)` | Closed-form probit average marginal effects |

# Examples

```julia
using MacroEconometricModels, Random, LinearAlgebra
sim = dgp_var(MersenneTwister(11); T=200)
fit = estimate_var(sim.Y, 1)
maximum(abs.(fit.Sigma - sim.Sigma))
est_irf = compute_irf(fit, Matrix{Float64}(I, 3, 3), 11)
maximum(abs.(est_irf - var_irf(sim.A, sim.B0, 10)))
```

# See also

* [Data Management](data.md) - observed datasets complementing simulated ones
* [Shared Kernel](core.md) - innovation-accounting estimators the population moments validate
* [Vector Autoregression](../multivariate/var.md) - OLS recovery on `dgp_var` draws
* [ARIMA Models](../univariate/arima.md) - recovery on `dgp_arima` draws
* [Hypothesis Tests](../testing/teststat.md) - size/power checks on `dgp_unit_root_pair`
