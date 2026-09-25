---
type: Feature
title: Nonlinear Time Series (Threshold, STAR, Markov-Switching)
description: Two-regime threshold and SETAR models, smooth-transition autoregression, and Markov-switching regression/MS-AR with linearity tests and forecasts.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nonlinear
tags:
  - nonlinear
  - threshold
  - setar
  - star
  - markov-switching
  - regime-switching
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/nonlinear-statespace
  at: 2026-09-25T01:09:05Z
sources:
  - id: nonlinear-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nonlinear.md
    title: Nonlinear Time Series docs page
  - id: nonlinear-threshold
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/threshold.jl
    title: Threshold least squares, SETAR, Hansen linearity test
  - id: nonlinear-star
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/star.jl
    title: Smooth-transition autoregression (STAR)
  - id: nonlinear-ms
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/markov_switching.jl
    title: Markov-switching regression and MS-AR
  - id: nonlinear-types
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl
    title: Nonlinear model and forecast types
---

# Summary

The `nonlinear` module fits regime-switching dynamics in the conditional mean: two-regime threshold least squares and self-exciting threshold autoregression (SETAR) with Hansen (1996) linearity tests and Hansen (2000) threshold confidence intervals, smooth-transition autoregression (STAR) with LSTR1/LSTR2/ESTR transitions and Teräsvirta transition selection, and Markov-switching regression plus Hamilton (1989) mean-switching MS-AR estimated via the Hamilton filter, Kim smoother, and EM with ML polish. All fits integrate with `report`, `refs`, `forecast`, and `plot_result`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_threshold](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/threshold.jl) | `estimate_threshold(y, X, q; trim=0.15, linearity=true, reps=1000, ci_level=0.95, het=false, rng)` | Two-regime threshold regression by grid search over order statistics of `q`; returns `ThresholdModel` |
| [estimate_setar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/threshold.jl) | `estimate_setar(y, p, d=1; trim, linearity, reps, ci_level, het, seed, rng)` | Self-exciting threshold AR with delay `d` (Int, range, or `:auto`); returns `ThresholdModel` |
| [hansen_linearity_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/threshold.jl) | `hansen_linearity_test(y, X, q; trim=0.15, reps=1000, seed, rng)` | Hansen (1996) sup-LM/sup-Wald test with fixed-regressor bootstrap p-values |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/threshold.jl) | `forecast(m::ThresholdModel, h; reps=1000, level=0.90, seed, rng)` | Bootstrap-simulation SETAR forecast with percentile bands (SETAR fits only) |
| [estimate_star](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/star.jl) | `estimate_star(y, p; s=nothing, d=1, type=:auto, n_gamma=15, n_c=15)` | STAR by NLS with grid starts and L-BFGS; `:auto` runs Teräsvirta selection |
| [star_linearity_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/star.jl) | `star_linearity_test(y, p; s=nothing, d=1)` | Luukkonen–Saikkonen–Teräsvirta LM3 linearity test (chi-square and F forms) |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/star.jl) | `forecast(m::STARModel, h; reps=1000, level=0.90, seed, rng)` | Bootstrap-simulation forecast of a self-exciting STAR model |
| [estimate_ms](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/markov_switching.jl) | `estimate_ms(y, X; k_regimes=2, switching_variance=true, max_iter=500, tol=1e-8, xnames)` | Markov-switching regression via EM plus ML polish; intercept-only call available |
| [estimate_ms_ar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/markov_switching.jl) | `estimate_ms_ar(y, p; k_regimes=2, switching_variance=false, max_iter=1000, yname="y")` | Hamilton (1989) mean-switching MS-AR(p) on the K^(p+1) expanded state space |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/markov_switching.jl) | `forecast(m::MSRegModel, h; reps=1000, level=0.90, seed, rng)` | Exact analytic mean forecast for `:ms_ar` with simulated bands |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/markov_switching.jl) | `forecast(m::MSRegModel, X_new; reps=1000, level=0.90, seed, rng)` | Forecast for `:regression` models given future regressors `X_new` |
| [predict](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl) | `predict(m::MSRegModel; probs=:smoothed)` | Regime-weighted conditional mean under smoothed or `:filtered` probabilities |
| [ThresholdModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl) | `ThresholdModel` (struct) | Fit object: threshold, regime coefficients, CI, attached linearity test |
| [STARModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl) | `STARModel` (struct) | Fit object: regime coefficients, transition slope/location, LM3 stats |
| [MSRegModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl) | `MSRegModel` (struct) | Fit object: regime means, transition matrix, filtered/smoothed probabilities |
| [HansenLinearityTest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonlinear/types.jl) | `HansenLinearityTest` (struct) | sup-LM/sup-Wald statistics with bootstrap p-values and `gamma_sup` |

# Examples

```julia
using MacroEconometricModels

# SETAR(2; 1, 1): AR(1) in each regime, threshold on y[t-1]
m = estimate_setar(y, 1, 1)
report(m)

# Test linearity first: Hansen (1996) sup-LM/sup-Wald, bootstrap p-values
X = hcat(ones(length(y) - 1), y[1:end-1])
lt = hansen_linearity_test(y[2:end], X, y[1:end-1]; reps=500)

# Bootstrap-simulation forecast with 90% bands
f = forecast(m, 8; reps=1000)
```

# See also

- /nonlinear-statespace/statespace.md
- /univariate/arima.md
- /multivariate/var.md
- /testing/tests.md
