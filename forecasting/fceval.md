---
type: Feature
title: Forecast Evaluation and Combination
description: Model-agnostic accuracy metrics, forecast-comparison tests, and combination schemes for point forecasts.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/fceval
tags:
  - forecast-evaluation
  - diebold-mariano
  - forecast-combination
  - accuracy-metrics
status: draft
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/forecasting
  at: 2026-09-25T01:31:57Z
sources:
  - id: forecast-evaluation-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/forecast_evaluation.md
    title: Forecast Evaluation & Combination docs page
  - id: fceval-metrics
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/metrics.jl
    title: Point accuracy metrics implementation
  - id: fceval-tests
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/tests.jl
    title: Forecast-comparison tests implementation
  - id: fceval-combine
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/combine.jl
    title: Forecast combination implementation
  - id: fceval-types
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl
    title: Forecast evaluation result types
---

# Summary

The `fceval` module scores point forecasts without touching any forecast type: every function consumes a plain vector of realized values plus a vector or matrix of forecasts, so output from a VAR, ARIMA, MIDAS regression, or a spreadsheet is scored the same way. It provides accuracy metrics (ME, MAE, RMSE, MAPE, sMAPE, MASE, Theil U1/U2, and the Theil MSE bias/variance/covariance decomposition), the Diebold-Mariano equal-accuracy test with the Harvey-Leybourne-Newbold small-sample correction, the Clark-West test for nested models, the Mincer-Zarnowitz efficiency regression, the Harvey-Leybourne-Newbold encompassing test, and three combination schemes (equal, Bates-Granger inverse-MSE, Granger-Ramanathan constrained least squares). Forecast errors follow the convention `e = actual - forecast`; the DM and Clark-West tests take error series while the other functions take forecasts alongside actuals.

# Functions

| Function | Signature | Role |
|---|---|---|
| [forecast_evaluate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/metrics.jl) | `forecast_evaluate(actual, fc; seasonal_period=1, insample=nothing, model_names=nothing) -> ForecastEvaluation` | Accuracy metrics plus Theil MSE decomposition for one forecast (vector) or several (T x M matrix) |
| [diebold_mariano](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/tests.jl) | `diebold_mariano(e1, e2; h=1, loss=:se, hln=true, kernel=:rectangular, alternative=:two_sided) -> DMTestResult` | Equal-predictive-accuracy test on two error series; invalid for nested models |
| [clark_west](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/tests.jl) | `clark_west(e_small, e_big, f_adj; h=1, alternative=:greater) -> ClarkWestResult` | Adjusted-MSPE test for nested models; third argument is the gap between the two point forecasts |
| [mincer_zarnowitz](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/tests.jl) | `mincer_zarnowitz(actual, fc; lags=0, kernel=:bartlett) -> MincerZarnowitzResult` | Efficiency regression `y = a + b*fc` with joint Wald test of `(a, b) = (0, 1)` |
| [forecast_encompassing](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/tests.jl) | `forecast_encompassing(actual, fc1, fc2; lags=0, kernel=:bartlett) -> ForecastEncompassingResult` | Tests `b2 = 0` in `y = a + b1*fc1 + b2*fc2`; non-rejection means fc1 encompasses fc2 |
| [combine_forecasts](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/combine.jl) | `combine_forecasts(F, actual; method=:equal, model_names=nothing) -> ForecastCombination` | Blends forecast columns into one series; Granger-Ramanathan weights may be negative |
| [ForecastEvaluation](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct ForecastEvaluation{T<:AbstractFloat}` | Accuracy table: `models`, `metrics`, `values`, `decomp`, `n` |
| [DMTestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct DMTestResult{T<:AbstractFloat} <: StatsAPI.HypothesisTest` | DM test result: `statistic`, `pvalue`, `dbar`, `lrvar`, `h`, `loss`, `hln`, `alternative`, `T_obs` |
| [ClarkWestResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct ClarkWestResult{T<:AbstractFloat} <: StatsAPI.HypothesisTest` | Clark-West result: `statistic`, `pvalue`, `fbar`, `lrvar`, `h`, `alternative`, `T_obs` |
| [MincerZarnowitzResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct MincerZarnowitzResult{T<:AbstractFloat} <: StatsAPI.HypothesisTest` | Efficiency result: `a`, `b`, `se`, `wald`, `pvalue_wald`, `fstat`, `pvalue_f`, `lags`, `kernel`, `T_obs` |
| [ForecastEncompassingResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct ForecastEncompassingResult{T<:AbstractFloat} <: StatsAPI.HypothesisTest` | Encompassing result: `b1`, `b2`, `se_b2`, `tstat`, `pvalue`, `lags`, `kernel`, `T_obs` |
| [ForecastCombination](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/fceval/types.jl) | `struct ForecastCombination{T<:AbstractFloat}` | Combination result: `weights`, `combined`, `method`, `mse`, `models` |

# Examples

```julia
using MacroEconometricModels
# actual: realized values; F: T x M matrix of competing forecasts
ev = forecast_evaluate(actual, F; model_names=["AR(2)", "Random walk", "Mean"])
report(ev)
dm = diebold_mariano(actual .- F[:, 1], actual .- F[:, 2]; h=1, loss=:se)
report(dm)
comb = combine_forecasts(F, actual; method=:granger_ramanathan)
report(comb)
```

# See also

- /forecasting/nowcast.md
- /multivariate/var.md
- /univariate/arima.md
