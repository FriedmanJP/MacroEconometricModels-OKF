---
type: Feature
title: Hypothesis Tests (teststat)
description: Unit root, cointegration, structural break, panel, and post-estimation diagnostic tests with a uniform StatsAPI result interface.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/teststat
tags:
  - testing
  - unit-root
  - cointegration
  - structural-breaks
  - panel
  - diagnostics
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/testing
  at: 2026-09-25T01:33:49Z
sources:
  - id: tests-overview
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests.md
    title: Hypothesis Tests overview and decision tables
  - id: tests-unitroot
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_unitroot.md
    title: Unit Root and Cointegration (ADF, KPSS, PP, ZA, Ng-Perron, Johansen)
  - id: tests-unitroot-advanced
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_unitroot_advanced.md
    title: Advanced Unit Root Tests (Fourier, DF-GLS, HEGY, LM, bubbles)
  - id: tests-cointegration
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_cointegration.md
    title: Residual-Based Cointegration Tests
  - id: tests-breaks
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_breaks.md
    title: Structural Breaks (Andrews, Bai-Perron, factor breaks, Gregory-Hansen)
  - id: tests-panel
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_panel.md
    title: Panel Tests (panel unit roots, panel cointegration, PVAR diagnostics)
  - id: tests-diagnostics
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/tests_diagnostics.md
    title: Model Diagnostics (Granger, normality, ARCH, BDS, EDF, model comparison)
  - id: src-teststat
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/teststat
    title: teststat module source tree (53 files)
---

# Summary

The `teststat` module (53 source files, the second-largest in the package) is the
shared battery of hypothesis tests used before estimation (integration order,
cointegrating rank, structural stability, panel stationarity) and after
estimation (Granger causality, residual normality, ARCH effects, independence,
distributional fit, nested model comparison). Every result type implements the
StatsAPI.jl interface (`statistic` via field access, `pvalue`, `nobs`, `dof`),
descends from `StatsAPI.HypothesisTest`, and renders with `report(result)`; most
unit root, cointegration, break, and panel results share the
`AbstractUnitRootTest` branch. Two results are not hypothesis tests:
`is_stationary` returns an eigenvalue diagnostic with no p-value, and
`normality_test_suite` returns a container of individual results.

# Functions

The table is capped at the 50 most important entry points, grouped by test
family. Omitted: the portmanteau trio (`ljung_box_test`, `box_pierce_test`,
`durbin_watson_test`, documented on the Spectral Analysis page), the group and
rank comparisons (`equality_test`, `cor_test`), `fisher_johansen_test`,
`panel_unit_root_summary`, `pvar_lag_selection`, and internal helpers. The
regression diagnostics (`chow_test`, `cusum_test`, `reset_test`, `white_test`,
and others) live in the `reg` module, not here.

## Unit root

| Function | Signature | Role |
|----------|-----------|------|
| [adf_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/adf.jl) | `adf_test(y; lags=:aic, max_lags=nothing, regression=:constant)` | Baseline Augmented Dickey-Fuller unit root test with MacKinnon p-values |
| [kpss_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/kpss.jl) | `kpss_test(y; regression=:constant, bandwidth=:auto)` | Stationarity-null test complementing ADF |
| [pp_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pp.jl) | `pp_test(y; regression=:constant, bandwidth=:auto)` | Non-parametric Phillips-Perron correction, no lag order needed |
| [za_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/za.jl) | `za_test(y; regression=:both, trim=0.15, lags=:aic, max_lags=nothing, outlier=:io)` | Zivot-Andrews unit root test with one endogenous break |
| [ngperron_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/ngperron.jl) | `ngperron_test(y; regression=:constant)` | GLS-detrended MZa/MZt/MSB/MPT suite, best size in small samples |
| [fourier_adf_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/fourier.jl) | `fourier_adf_test(y; regression=:constant, fmax=3, lags=:aic, max_lags=nothing, trim=0.15)` | ADF with trigonometric terms absorbing smooth breaks of unknown form |
| [fourier_kpss_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/fourier.jl) | `fourier_kpss_test(y; regression=:constant, fmax=3, bandwidth=nothing)` | Stationarity-null counterpart of the Fourier ADF test |
| [dfgls_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/dfgls.jl) | `dfgls_test(y; regression=:constant, lags=:aic, max_lags=nothing)` | GLS-detrended ADF with near-optimal power against local alternatives |
| [ers_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/dfgls.jl) | `ers_test(y; trend=false)` | Elliott-Rothenberg-Stock point-optimal P_T test |
| [hegy_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/hegy.jl) | `hegy_test(y; frequency=4, deterministic=:const_trend_seas, lags=:auto)` | Seasonal unit roots frequency by frequency (quarterly/monthly) |
| [lm_unitroot_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/lm_unitroot.jl) | `lm_unitroot_test(y; breaks=0, regression=:level, lags=:aic, max_lags=nothing, trim=0.15)` | LM unit root test with 0/1/2 breaks allowed under the null |
| [adf_2break_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/adf_2break.jl) | `adf_2break_test(y; model=:level, lags=:aic, max_lags=nothing, trim=0.10)` | ADF with two endogenous breaks in level and slope |
| [sadf_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/bubble.jl) | `sadf_test(y; r0=:auto, adflag=0, mc_reps=999, cv=:asymptotic, seed=20240716)` | Right-tailed sup-ADF test for a single explosive bubble with date-stamping |
| [gsadf_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/bubble.jl) | `gsadf_test(y; r0=:auto, adflag=0, mc_reps=999, cv=:asymptotic, seed=20240716)` | Generalized sup-ADF test detecting multiple explosive episodes |
| [unit_root_summary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/convenience.jl) | `unit_root_summary(y; tests=[:adf, :kpss, :pp], regression=:constant)` | Runs several tests on one series and synthesizes an ADF/KPSS verdict |
| [test_all_variables](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/convenience.jl) | `test_all_variables(Y; test=:adf, kwargs...)` | Applies one unit root test to every column of a matrix |

## Cointegration

| Function | Signature | Role |
|----------|-----------|------|
| [johansen_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/johansen.jl) | `johansen_test(Y, p; deterministic=:constant, significance=0.05)` | System cointegrating rank via trace and maximum-eigenvalue statistics |
| [engle_granger_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/engle_granger.jl) | `engle_granger_test(y, X; trend=:constant, lags=:aic, max_lags=nothing)` | Two-step ADF on cointegrating residuals; null is no cointegration |
| [phillips_ouliaris_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/phillips_ouliaris.jl) | `phillips_ouliaris_test(y, X; trend=:constant, kernel=:bartlett, bandwidth=:nw)` | Semiparametric Z_t/Z_alpha residual tests; no lag order needed |
| [hansen_instability_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/hansen_instability.jl) | `hansen_instability_test(m::CointRegModel)` | L_c stability test on a fitted cointegrating regression |
| [park_added_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/park_added.jl) | `park_added_test(m::CointRegModel; q_add=2, kernel=:bartlett, bandwidth=:nw)` | H(p,q) superfluous-trend test of genuine vs spurious cointegration |

## Structural breaks

| Function | Signature | Role |
|----------|-----------|------|
| [andrews_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/andrews.jl) | `andrews_test(y, X; test=:supwald, trimming=0.15)` | Single unknown break; nine sup/exp/mean Wald, LR, and LM variants |
| [bai_perron_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/bai_perron.jl) | `bai_perron_test(y, X; max_breaks=5, trimming=0.15, criterion=:bic)` | Multiple breaks via dynamic programming with BIC/LWZ selection |
| [factor_break_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/factor_break.jl) | `factor_break_test(X, r; method=:breitung_eickmeier, kwargs...)` | Factor loading instability (pooled LM, sup-LM, sup-Wald) |
| [gregory_hansen_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/gregory_hansen.jl) | `gregory_hansen_test(Y; model=:C, lags=:aic, max_lags=nothing, trim=0.15)` | Cointegration allowing one regime shift in the long-run relationship |

## Diagnostics

| Function | Signature | Role |
|----------|-----------|------|
| [is_stationary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/stationarity.jl) | `is_stationary(model::VARModel)` | VAR companion eigenvalue stability check (no p-value) |
| [granger_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/granger.jl) | `granger_test(model::VARModel, cause, effect)` | Pairwise or block Wald test of predictive causality |
| [granger_test_all](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/granger.jl) | `granger_test_all(model::VARModel)` | Granger causality for every ordered variable pair |
| [normality_test_suite](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/normality.jl) | `normality_test_suite(U)` / `normality_test_suite(model::VARModel)` | Seven multivariate normality tests in one call |
| [arch_lm_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `arch_lm_test(y, q=5)` | Engle ARCH-LM test for remaining conditional heteroskedasticity |
| [ljung_box_squared](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `ljung_box_squared(z, K=10)` | Ljung-Box test on squared residuals as an ARCH check |
| [bds_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/bds.jl) | `bds_test(y; m=2:6, eps_frac=0.7, bootstrap=0, seed=1234)` | Independence test against any departure from i.i.d. |
| [edf_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/edf.jl) | `edf_test(y; dist=:normal, test=:ad, params=:estimate, theta=nothing)` | EDF goodness-of-fit (KS, AD, CvM, Watson) against parametric families |
| [variance_ratio_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/variance_ratio.jl) | `variance_ratio_test(y; q=[2,4,8,16], method=:lomackinlay, bootstrap=0, robust=true, boot_weights=:rademacher, seed=1234)` | Lo-MacKinlay/Wright random-walk tests, optionally wild-bootstrapped |
| [lr_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/model_comparison.jl) | `lr_test(m1, m2)` | Likelihood ratio test for nested models |
| [lm_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/model_comparison.jl) | `lm_test(m1, m2)` | Lagrange multiplier (score) test for nested models |

## Panel

| Function | Signature | Role |
|----------|-----------|------|
| [llc_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/llc.jl) | `llc_test(X; deterministic=:constant, lags=:auto, max_lags=nothing, criterion=:aic, cs_demean=false)` | Levin-Lin-Chu pooled panel unit root test under a common root |
| [ips_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/ips.jl) | `ips_test(X; deterministic=:constant, lags=:auto, max_lags=nothing, criterion=:aic, cs_demean=false)` | Im-Pesaran-Shin averaged per-unit ADF test under heterogeneous roots |
| [breitung_panel_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/breitung_panel.jl) | `breitung_panel_test(X; deterministic=:constant, lags=0, cs_demean=false)` | Bias-free pooled panel unit root test via forward orthogonal deviations |
| [fisher_panel_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/fisher_panel.jl) | `fisher_panel_test(X; base=:adf, combine=:mw, lags=:auto, deterministic=:constant, cs_demean=false)` | Maddala-Wu/Choi combination of per-unit ADF or PP p-values |
| [hadri_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/hadri.jl) | `hadri_test(X; deterministic=:constant, hetero=true, cs_demean=false)` | Panel KPSS: LM test with a stationarity null |
| [panic_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/panic.jl) | `panic_test(X; r=:auto, method=:pooled)` | Bai-Ng factor decomposition into common and idiosyncratic components |
| [pesaran_cips_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pesaran_cips.jl) | `pesaran_cips_test(X; lags=:auto, deterministic=:constant)` | Cross-sectionally augmented IPS test robust to a common factor |
| [moon_perron_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/moon_perron.jl) | `moon_perron_test(X; r=:auto)` | Factor-adjusted pooled panel unit root test with bias correction |
| [pedroni_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pedroni.jl) | `pedroni_test(pd, y, xs...; trend=:constant, lags=:auto, adf_lags=2)` | Residual-based panel cointegration tests on PanelData |
| [kao_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/kao.jl) | `kao_test(pd, y, xs...; lags=:auto, kernel_lags=:auto)` | Homogeneous panel cointegration test on PanelData |
| [westerlund_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/westerlund.jl) | `westerlund_test(pd, y, xs...; trend=:constant, lags=1, leads=0, lrwindow=2, bootstrap=0, seed=20240716)` | Error-correction-based panel cointegration tests |
| [dh_causality_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/dumitrescu_hurlin.jl) | `dh_causality_test(pd, x, y; p=1, bootstrap=0, seed=1234)` | Dumitrescu-Hurlin heterogeneous-panel Granger non-causality |
| [pvar_hansen_j](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_hansen_j.jl) | `pvar_hansen_j(model::PVARModel)` | Hansen J-test of PVAR overidentifying restrictions |
| [pvar_mmsc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/pvar_mmsc.jl) | `pvar_mmsc(model::PVARModel; hq_criterion=2.1)` | Andrews-Lu MMSC lag/order selection criteria for PVARs |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
cpi = filter(isfinite, fred[:, "CPIAUCSL"])
result = adf_test(cpi; lags=:aic, regression=:constant)
report(result)
```

The statistic (≈ 2.15) lies far above the 5% critical value (≈ −2.85), so the
test fails to reject the unit root null — the expected verdict for a price
level. Confirm with `kpss_test`, whose null is stationarity, before differencing.

# See also

- [/multivariate/var.md](/multivariate/var.md) — VAR estimation whose lagged dynamics these tests validate
- [/multivariate/vecm.md](/multivariate/vecm.md) — VECM estimation consuming Johansen cointegrating vectors
- [/univariate/arima.md](/univariate/arima.md) — univariate models specified after unit root screening
- [/panel/index.md](/panel/index.md) — panel estimation validated by the panel tests
