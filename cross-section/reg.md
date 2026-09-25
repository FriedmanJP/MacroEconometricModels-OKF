---
type: Feature
title: Regression (OLS, IV, and Limited Dependent Variables)
description: Single-equation cross-sectional estimation from OLS/WLS and IV/2SLS through logit/probit, ordered and multinomial choice, Tobit, Heckman, count, quantile, RDD, penalized, and robust estimators with residual, stability, and influence diagnostics.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/reg
tags:
  - cross-section
  - regression
  - ols
  - iv
  - discrete-choice
  - diagnostics
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/cross-section
  at: 2026-09-25T01:30:07Z
sources:
  - id: regression-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/regression.md
    title: Linear Regression docs page
  - id: binary-choice-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/binary_choice.md
    title: Binary Choice Models docs page
  - id: ordered-multinomial-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/ordered_multinomial.md
    title: Ordered and Multinomial Models docs page
  - id: src-reg
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/reg
    title: src/reg module source
---

# Summary

The `reg` module is the package's cross-sectional regression toolkit: OLS/WLS with HC0-HC3, cluster, and Conley spatial-HAC covariance (`estimate_reg`, `conley_se`), IV/2SLS with LIML, Fuller, and k-class variants plus Anderson-Rubin weak-instrument-robust inference (`estimate_iv`, `anderson_rubin_test`, `anderson_rubin_ci`), binary logit/probit by IRLS (`estimate_logit`, `estimate_probit`), ordered logit/probit and multinomial logit by Newton-Raphson (`estimate_ologit`, `estimate_oprobit`, `estimate_mlogit`), quantile regression (`estimate_qreg`), regression discontinuity with CCT robust bias-corrected inference (`estimate_rdd`), ridge/LASSO/elastic-net penalized regression (`estimate_ridge`, `estimate_lasso`, `estimate_elastic_net`), stepwise and general-to-specific selection (`select_variables`), Tobit and truncated regression (`estimate_tobit`, `estimate_truncreg`), Heckman selection (`estimate_heckman`), Poisson and Negative Binomial counts (`estimate_poisson`, `estimate_nbreg`), Huber/bisquare M- and MM-estimation (`estimate_robust`), and the wild cluster bootstrap for few clusters (`wild_cluster_bootstrap`).
Every estimator returns a typed result (`RegModel`, `LogitModel`, `ProbitModel`, `OrderedLogitModel`, `OrderedProbitModel`, `MultinomialLogitModel`, `QuantileRegModel`, `RDDResult`, `PenalizedRegModel`, `TobitModel`, `TruncRegModel`, `HeckmanModel`, `PoissonModel`, `NegBinModel`, `RobustRegModel`) printable via `report()` and served through the StatsAPI interface, with per-family gaps: `r2` exists only for OLS/penalized/count fits, `loglikelihood` is missing for quantile/robust/RDD, `vcov`/`stderror`/`confint` are missing for penalized fits, and `RDDResult` carries no StatsAPI methods.
Post-estimation covers marginal effects for the binary/ordered/multinomial, Tobit, and count families (not Heckman, quantile, robust, penalized, or RDD), odds and incidence-rate ratios, classification tables, VIF, overdispersion, Brant proportional-odds, and Hausman-McFadden IIA tests, plus OLS residual diagnostics (White, Breusch-Pagan, Glejser, Harvey, Breusch-Godfrey, RESET), stability diagnostics (recursive residuals, CUSUM/CUSUMSQ, Chow), and Belsley-Kuh-Welsch influence statistics.
Symbol-based `CrossSectionData` dispatches for `estimate_reg`, `estimate_iv`, `estimate_logit`, and `estimate_probit` live in `src/data/convert.jl`; kernel-HAC long-run variance (`lrvar`, `lrcov`, `lrcov_oneside`, `varhac`) lives in `src/core` and is covered by the infrastructure domain.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_reg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/estimation.jl) | `estimate_reg(y, X; cov_type=:hc1, weights=nothing, varnames=nothing, clusters=nothing, coords=nothing, cutoff=0.0, conley_kernel=:bartlett, conley_metric=:euclidean, time=nothing, time_cutoff=0, conley_psd=true)` | OLS (or WLS with `weights`) with classical, HC0-HC3, cluster, or Conley covariance; returns `RegModel` |
| [estimate_iv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/iv.jl) | `estimate_iv(y, X, Z; endogenous, method=:tsls, k=nothing, fuller_a=1.0, cov_type=:hc1, varnames=nothing)` | 2SLS, LIML, Fuller, or k-class IV with first-stage F, Sargan-Hansen, Cragg-Donald, Kleibergen-Paap, and Stock-Yogo diagnostics |
| [estimate_logit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/logit.jl) | `estimate_logit(y, X; cov_type=:ols, varnames=nothing, clusters=nothing, maxiter=100, tol=1e-8)` | Binary logit by IRLS/Fisher scoring with separation warning; returns `LogitModel` |
| [estimate_probit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/probit.jl) | `estimate_probit(y, X; cov_type=:ols, varnames=nothing, clusters=nothing, maxiter=100, tol=1e-8)` | Binary probit by Fisher scoring; returns `ProbitModel` |
| [estimate_ologit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/ordered.jl) | `estimate_ologit(y, X; cov_type=:ols, varnames=nothing, clusters=nothing, maxiter=200, tol=1e-8)` | Ordered (proportional-odds) logit by Newton-Raphson; `X` must exclude the intercept |
| [estimate_oprobit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/ordered.jl) | `estimate_oprobit(y, X; cov_type=:ols, varnames=nothing, clusters=nothing, maxiter=200, tol=1e-8)` | Ordered probit by Newton-Raphson; `X` must exclude the intercept |
| [estimate_mlogit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/multinomial.jl) | `estimate_mlogit(y, X; cov_type=:ols, varnames=nothing, clusters=nothing, maxiter=200, tol=1e-8)` | Multinomial (softmax) logit by Newton-Raphson; `X` must include an intercept column |
| [estimate_qreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/quantile.jl) | `estimate_qreg(y, X, tau=0.5; se=:iid, varnames=nothing, n_boot=500, seed=nothing, rng=Random.default_rng(), alpha=0.05)` | Quantile regression at one or more quantiles with `:iid`, `:robust`, or `:boot` standard errors |
| [estimate_rdd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/rdd.jl) | `estimate_rdd(y, running; cutoff=0.0, fuzzy=nothing, kernel=:triangular, p=1, h=nothing, b=nothing, level=0.95)` | Sharp or fuzzy regression discontinuity with CCT robust bias-corrected inference |
| [estimate_elastic_net](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/penalized.jl) | `estimate_elastic_net(y, X; alpha=1.0, lambda=:cv, nlambda=100, lambda_min_ratio=1e-4, select=:cv, cv=:kfold, nfolds=10, adaptive=false, adaptive_gamma=1.0, post=false, standardize=true, seed=1234, tol=1e-9, maxit=100000)` | Elastic-net penalized regression over a warm-started lambda path with CV or IC selection |
| [estimate_lasso](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/penalized.jl) | `estimate_lasso(y, X; kwargs...)` | LASSO as `estimate_elastic_net` with `alpha=1`; sparse solutions |
| [estimate_ridge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/penalized.jl) | `estimate_ridge(y, X; kwargs...)` | Ridge as `estimate_elastic_net` with `alpha=0`; closed-form dense shrinkage |
| [select_variables](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/selection.jl) | `select_variables(y, X; method=:bidirectional, criterion=:pvalue, p_enter=0.05, p_remove=0.10, p_gets=0.05, diag_level=0.05, bg_lags=1, keep=nothing, varnames=nothing)` | Forward/backward/bidirectional stepwise, best-subset, or GETS specification search; returns `SelectionResult` |
| [estimate_tobit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/tobit.jl) | `estimate_tobit(y, X; lower=0.0, upper=Inf, dist=:normal, varnames=nothing, maxiter=1000, tol=1e-10)` | Censored (Tobit) regression by MLE in the Olsen reparameterization |
| [estimate_truncreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/tobit.jl) | `estimate_truncreg(y, X; lower=0.0, upper=Inf, varnames=nothing, maxiter=1000, tol=1e-10)` | Truncated-normal regression by MLE; every `y` must lie strictly inside the bounds |
| [estimate_heckman](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/heckman.jl) | `estimate_heckman(y, X, d, Z; method=:twostep, outcome_names=nothing, select_names=nothing, maxiter=1000, tol=1e-10)` | Heckman selection by two-step Heckit or full-information MLE |
| [estimate_poisson](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/count.jl) | `estimate_poisson(y, X; offset=nothing, exposure=nothing, cov_type=:robust, varnames=nothing, clusters=nothing, maxiter=100, tol=1e-10)` | Poisson quasi-MLE with GMT sandwich errors by default |
| [estimate_nbreg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/count.jl) | `estimate_nbreg(y, X; offset=nothing, exposure=nothing, varnames=nothing, maxiter=1000, tol=1e-10)` | Negative Binomial 2 with jointly estimated dispersion `alpha` |
| [estimate_robust](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/robust.jl) | `estimate_robust(y, X; psi=:huber, method=:m, k=nothing, maxiter=50, tol=1e-6, scale_update=:mad, rng=Random.default_rng(), seed=nothing, n_resample=500, varnames=nothing)` | Huber/bisquare M-estimation or Yohai MM-estimation with Huber-Ronchetti sandwich covariance |
| [estimate_robust](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/robust.jl) | `estimate_robust(d::CrossSectionData, y, xs; add_intercept=true, kwargs...)` | Robust regression from a `CrossSectionData` container by column name |
| [conley_se](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/covariance.jl) | `conley_se(m::RegModel; coords, cutoff, kernel=:bartlett, metric=:euclidean, time=nothing, time_cutoff=0, time_kernel=:bartlett, psd=true)` | Conley spatial-HAC standard errors for a fitted `RegModel` |
| [wild_cluster_bootstrap](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/wildboot.jl) | `wild_cluster_bootstrap(model::RegModel, coefficient, null_value=0.0; clusters, kwargs...)` | Cameron-Gelbach-Miller wild cluster bootstrap with exact enumeration for few clusters |
| [wild_cluster_bootstrap](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/wildboot.jl) | `wild_cluster_bootstrap(model::PanelRegModel, coefficient, null_value=0.0; clusters=nothing, kwargs...)` | Wild cluster bootstrap on a fixed-effects panel fit, defaulting clusters to entity ids |
| [anderson_rubin_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/anderson_rubin.jl) | `anderson_rubin_test(model, beta0; cov_type=nothing, clusters=nothing)` | Anderson-Rubin weak-instrument-robust test of one or more endogenous coefficients |
| [anderson_rubin_ci](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/anderson_rubin.jl) | `anderson_rubin_ci(model; level=0.95, n_grid=1001, span=20, grid=nothing, cov_type=nothing, clusters=nothing)` | AR confidence set by test inversion; may be unbounded, disjoint, empty, or the whole line |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/margins.jl) | `marginal_effects(m::Union{LogitModel,ProbitModel}; type=:ame, at=nothing, conf_level=0.95)` | AME/MEM/MER with delta-method SEs; discrete change for `{0,1}` regressors |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/ordered.jl) | `marginal_effects(m::Union{OrderedLogitModel,OrderedProbitModel})` | Average marginal effects as a `K x J` variables-by-categories matrix |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/multinomial.jl) | `marginal_effects(m::MultinomialLogitModel)` | Average marginal effects with full-covariance delta-method SEs; no intercept row |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/tobit.jl) | `marginal_effects(m::TobitModel; which=:unconditional, conf_level=0.95)` | McDonald-Moffitt decomposition: `:unconditional`, `:conditional`, or `:probability` |
| [marginal_effects](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/count.jl) | `marginal_effects(m::Union{PoissonModel,NegBinModel}; conf_level=0.95)` | Average marginal effects on the count mean; discrete change for binary regressors |
| [odds_ratio](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/margins.jl) | `odds_ratio(m::LogitModel; conf_level=0.95)` | Exponentiated logit coefficients with log-scale confidence intervals |
| [incidence_rate_ratio](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/count.jl) | `incidence_rate_ratio(m::Union{PoissonModel,NegBinModel}; conf_level=0.95)` | Exponentiated count-model coefficients in the shared `OddsRatio` container |
| [classification_table](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `classification_table(m::Union{LogitModel,ProbitModel}; threshold=0.5)` | Confusion matrix with accuracy, sensitivity, specificity, precision, and F1 |
| [vif](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `vif(m::RegModel)` | Variance inflation factors, one per non-intercept regressor |
| [dispersion_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/count.jl) | `dispersion_test(m::PoissonModel)` | Cameron-Trivedi NB2/NB1 overdispersion test on a Poisson fit |
| [brant_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/ordered.jl) | `brant_test(m::OrderedLogitModel)` | Brant proportional-odds test, overall plus per-variable, via `J-1` binary logits |
| [hausman_iia](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/multinomial.jl) | `hausman_iia(m::MultinomialLogitModel; omit_category::Int)` | Hausman-McFadden IIA test re-estimating without one category; needs `J >= 4` |
| [generalized_residuals](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/ordered.jl) | `generalized_residuals(m::Union{OrderedLogitModel,OrderedProbitModel})` | Length-`n` score residual vector for ordered models (Chesher-Irish) |
| [white_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `white_test(m::RegModel; cross_terms=true)` | White heteroskedasticity test; also accepts `(resid, X)` |
| [breusch_pagan_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `breusch_pagan_test(m::RegModel; studentized=true, het_regressors=nothing)` | Koenker studentized (default) or original Breusch-Pagan test; also accepts `(resid, X)` |
| [glejser_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `glejser_test(m::RegModel)` | Glejser F-test of `|resid|` on the regressors; also accepts `(resid, X)` |
| [harvey_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `harvey_test(m::RegModel)` | Harvey multiplicative-heteroskedasticity test on `log(resid^2)`; also accepts `(resid, X)` |
| [breusch_godfrey_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `breusch_godfrey_test(m::RegModel; lags=1)` | Breusch-Godfrey serial-correlation LM test with chi-squared and F forms |
| [reset_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/diagnostics.jl) | `reset_test(m::RegModel; powers=2:4)` | Ramsey RESET functional-form F-test on powers of the fitted values |
| [recursive_residuals](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/stability.jl) | `recursive_residuals(m::RegModel)` | Standardized Brown-Durbin-Evans recursive residuals, length `n-k` |
| [cusum_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/stability.jl) | `cusum_test(m::RegModel; level=0.05)` | CUSUM parameter-stability test against BDE significance lines |
| [cusumsq_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/stability.jl) | `cusumsq_test(m::RegModel; level=0.05)` | CUSUM-of-squares variance-stability test with Edgerton-Wells band |
| [chow_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/stability.jl) | `chow_test(m::RegModel, break_index; type=:breakpoint, level=0.05)` | Chow breakpoint or forecast test at known break date(s) |
| [influence_stats](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/reg/stability.jl) | `influence_stats(m::RegModel)` | Hat diagonals, studentized residuals, DFFITS, Cook's distance, and DFBETAS |

# Examples

```julia
using MacroEconometricModels
n = 200
X = hcat(ones(n), randn(n, 2))
y = X * [1.0, 2.0, -0.5] + 0.5 * randn(n)
m = estimate_reg(y, X; varnames=["(Intercept)", "x1", "x2"])
report(m)
```

# See also

* [Systems of Equations (SUR and 3SLS)](/cross-section/system.md) - joint estimation of multi-equation systems with correlated errors
* [Panel Regression](/panel/preg.md) - fixed/random-effects, panel IV, and panel logit/probit counterparts
* [Difference-in-Differences](/panel/did.md) - treatment-effect designs that use few-cluster bootstrap inference
