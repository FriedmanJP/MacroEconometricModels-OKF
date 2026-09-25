---
type: Feature
title: Statistical Identification (Non-Gaussian and Heteroskedastic SVAR)
description: Data-driven SVAR identification from non-Gaussian shocks or time-varying volatility without recursive, sign, or exclusion restrictions, with diagnostics and shock labelling.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nongaussian
tags:
  - svar
  - identification
  - non-gaussian
  - heteroskedasticity
  - multivariate
  - ica
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/multivariate
  at: 2026-09-25T01:16:20Z
sources:
  - id: nongaussian-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nongaussian.md
    title: Statistical Identification overview docs page
  - id: id-nongaussian-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/id_nongaussian.md
    title: Non-Gaussian Methods docs page
  - id: id-heteroskedastic-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/id_heteroskedastic.md
    title: Heteroskedasticity-Based Identification docs page
  - id: src-nongaussian
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nongaussian
    title: src/nongaussian module source
---

# Summary

The `nongaussian` module recovers the structural impact matrix `B0 = L Q` from higher-moment information instead of economic restrictions: at most one Gaussian shock (Darmois-Skitovich) or regime-varying variances identify the rotation.
Five nonparametric ICA estimators (FastICA, JADE, SOBI, distance covariance, HSIC), four parametric ML estimators (Student-t, mixture of normals, Pearson Type IV PML, skew-normal) plus the `identify_nongaussian_ml` dispatcher, and Keweloh/Lanne-Luoto moment GMM exploit non-Gaussianity; Markov-switching, GARCH, smooth-transition, and external-regime estimators plus Lewis (2021) model-free TVV GMM and the Bertsche-Braun SV-SVAR exploit heteroskedasticity.
Every result plugs into `irf`/`fevd`/`historical_decomposition` via a `method=` symbol, `label_shocks` resolves the column permutation and sign, and the `test_*` diagnostics check Gaussianity, independence, distinct variance ratios, and label stability before interpretation.

# Functions

| Function | Signature | Role |
|---|---|---|
| [identify_fastica](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `identify_fastica(model; contrast=:logcosh, approach=:deflation, max_iter=200, tol=1e-6, seed, rng)` | Negentropy-maximizing ICA (Hyvarinen 1999); returns `ICASVARResult` |
| [identify_jade](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `identify_jade(model; max_iter=100, tol=1e-6)` | Fourth-cumulant joint diagonalization (Cardoso & Souloumiac 1993) |
| [identify_sobi](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `identify_sobi(model; lags=1:12, max_iter=100, tol=1e-6)` | Second-order blind identification from autocovariances (Belouchrani et al. 1997) |
| [identify_dcov](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `identify_dcov(model; max_iter=200, tol=1e-6)` | Independence search via pairwise distance covariance (Matteson & Tsay 2017) |
| [identify_hsic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `identify_hsic(model; kernel=:gaussian, sigma=1.0, max_iter=200, tol=1e-6, seed, rng)` | Kernel-independence (HSIC) search with median-heuristic bandwidth |
| [identify_student_t](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `identify_student_t(model; max_iter=500, tol=1e-6)` | ML with shock-specific Student-t tails (Lanne, Meitz & Saikkonen 2017) |
| [identify_mixture_normal](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `identify_mixture_normal(model; n_components=2, max_iter=500, tol=1e-6)` | ML with two-component Gaussian-mixture shocks (Lanne & Lutkepohl 2010) |
| [identify_pml](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `identify_pml(model; max_iter=500, tol=1e-6)` | ML with normalized Pearson Type IV shocks (skewness + heavy tails) |
| [identify_skew_normal](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `identify_skew_normal(model; max_iter=500, tol=1e-6)` | ML with Azzalini skew-normal shocks |
| [identify_nongaussian_ml](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `identify_nongaussian_ml(model; distribution=:student_t, max_iter=500, tol=1e-6)` | Unified ML dispatcher over the four shock distributions |
| [identify_gmm_moments](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/gmm.jl) | `identify_gmm_moments(model; moments=:both, weighting=:two_step, se=:sandwich, hac=true, bandwidth=0, max_iter=100, tol=1e-8, n_starts=1, rng)` | Coskewness/cokurtosis GMM (Keweloh 2021) with Hansen J and sandwich SEs |
| [identify_lewis_tvv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/lewis_tvv.jl) | `identify_lewis_tvv(model; K=5, lags=1:K, weighting=:two_step, hac=true, bandwidth=0, n_starts=10, max_iter=100, tol=1e-8, rng)` | Model-free time-varying-volatility GMM (Lewis 2021); check `weak_id` |
| [identify_sv_svar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/sv_svar.jl) | `identify_sv_svar(Y, p; hetero, smoother=:ksc, maxiter=500, tol=1e-3, b_iter=5, gibbs_burn=5, gibbs_draws=100, init=:ols_chol, phi_init=0.9, s_init=0.2, theta_grid=12, c=1e-5, rng)` | EM-ML SV-SVAR with AR(1) log-volatilities (Bertsche & Braun 2022) |
| [identify_markov_switching](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `identify_markov_switching(model; n_regimes=2, max_iter=500, tol=1e-6, n_starts=5, rng)` | EM Markov-switching covariances with joint-ML `B0` (Lanne & Lutkepohl 2008) |
| [identify_garch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `identify_garch(model; max_iter=500, tol=1e-6)` | Iterative GARCH(1,1)-variance SVAR identification (Normandin & Phaneuf 2004) |
| [identify_smooth_transition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `identify_smooth_transition(model, transition_var; max_iter=500, tol=1e-6)` | Joint-ML logistic smooth-transition variances (Lutkepohl & Netsunajev 2017) |
| [identify_external_volatility](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `identify_external_volatility(model, regime_indicator; regimes=2)` | Sample-split identification on known regimes (Rigobon 2003) |
| [label_shocks](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/core/identification.jl) | `label_shocks(result; by=:restrictions, restrictions, B_ref, variables, shock_names, sign_convention=:positive_diagonal)` | Signed-permutation shock labelling by restrictions, max impact, or reference |
| [test_shock_gaussianity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_shock_gaussianity(result)` | Joint Jarque-Bera Gaussianity test over recovered shocks |
| [test_gaussian_shock_count](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_gaussian_shock_count(result; alpha=0.05)` | Sequential Holm-adjusted count; identification needs at most one Gaussian shock |
| [test_gaussian_vs_nongaussian](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_gaussian_vs_nongaussian(model; distribution=:student_t)` | LR test of Gaussian vs non-Gaussian shocks |
| [test_shock_independence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_shock_independence(result; max_lag=10, seed, rng)` | Portmanteau plus distance-covariance independence test (Fisher combined) |
| [test_label_stability](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_label_stability(model; method=:fastica, n_bootstrap=999, rng, transition_var, regime_indicator)` | Bootstrap column-match fraction for label stability (no p-value) |
| [test_lambda_distinct](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `test_lambda_distinct(result; pairs=:all)` | Distinct variance-ratio check for heteroskedastic identification |
| [ICASVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ica.jl) | `struct ICASVARResult{T} <: AbstractNonGaussianSVAR` | ICA fit: `B0`, `W`, `Q`, shocks, convergence, method-specific objective |
| [NonGaussianMLResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/ml.jl) | `struct NonGaussianMLResult{T} <: AbstractNonGaussianSVAR` | ML fit: `B0`, `Q`, logliks, `dist_params`, `se`, AIC/BIC |
| [NonGaussianGMMResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/gmm.jl) | `struct NonGaussianGMMResult{T} <: AbstractNonGaussianSVAR` | GMM fit: `B0`, Givens `theta`, sandwich vcov, Hansen `J` |
| [LewisTVVResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/lewis_tvv.jl) | `struct LewisTVVResult{T} <: AbstractNonGaussianSVAR` | TVV-GMM fit with `weak_id` identification flag |
| [SVSVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/sv_svar.jl) | `struct SVSVARResult{T} <: AbstractNonGaussianSVAR` | SV-SVAR EM fit: rotation, VAR slopes, SV params, smoothed variances |
| [MarkovSwitchingSVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `struct MarkovSwitchingSVARResult{T} <: AbstractNonGaussianSVAR` | Regime covariances, transition matrix, `Lambda` variance ratios |
| [GARCHSVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `struct GARCHSVARResult{T} <: AbstractNonGaussianSVAR` | GARCH-variance fit: per-shock GARCH params and conditional variances |
| [SmoothTransitionSVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `struct SmoothTransitionSVARResult{T} <: AbstractNonGaussianSVAR` | Logistic-transition fit: `gamma`, threshold, pole covariances |
| [ExternalVolatilitySVARResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/heteroskedastic.jl) | `struct ExternalVolatilitySVARResult{T} <: AbstractNonGaussianSVAR` | Known-regime fit: per-regime covariances and `Lambda` ratios |
| [IdentifiabilityTestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nongaussian/tests.jl) | `struct IdentifiabilityTestResult{T}` | Diagnostic verdict: statistic, p-value, `identified` flag, details |

# Examples

```julia
using MacroEconometricModels
model = estimate_var(Y, 2)
ica = identify_fastica(model; rng=MersenneTwister(11))
report(ica)
labeled = label_shocks(ica; by=:max_impact, variables=1:3,
                       shock_names=["output", "price", "mp"])
irfs = irf(model, 20; method=:fastica, rng=MersenneTwister(11))
report(irfs)
```

# See also

* [Vector Autoregression](/multivariate/var.md) - reduced-form estimation feeding every `identify_*` method
* [Local Projections](/multivariate/lp.md) - LP-IV external-instrument identification as an economic-restriction alternative
* [Multivariate GARCH](/multivariate/mgarch.md) - conditional covariance modelling behind GARCH-based identification
* [Vector Error Correction Models](/multivariate/vecm.md) - SVEC permanent/transitory identification for cointegrated systems
