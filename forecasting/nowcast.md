---
type: Feature
title: Nowcasting Mixed-Frequency Data
description: Real-time current-quarter estimates from ragged-edge panels via DFM, large BVAR, bridge equations, and news decomposition.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nowcast
tags:
  - nowcasting
  - mixed-frequency
  - dynamic-factor-model
  - bvar
  - bridge-equations
  - news-decomposition
status: approved
reviewed_by: chung9207
reviewed_at: 2026-09-25
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/forecasting
  at: 2026-09-25T01:31:57Z
sources:
  - id: nowcast-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nowcast.md
    title: Nowcasting shared-material docs page
  - id: nowcast-dfm-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nowcast_dfm.md
    title: DFM Nowcasting docs page
  - id: nowcast-bvar-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nowcast_bvar.md
    title: BVAR Nowcasting docs page
  - id: nowcast-bridge-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nowcast_bridge.md
    title: Bridge Equations docs page
  - id: nowcast-news-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nowcast_news.md
    title: News Decomposition docs page
  - id: nowcast-src
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nowcast
    title: Nowcasting source directory
---

# Summary

The `nowcast` module produces current-quarter estimates of quarterly aggregates from timely monthly indicators under the three defining challenges of the problem: mixed frequencies, ragged edges, and large cross-sections. All estimators share one data layout: a `T x N` matrix whose first `nM` columns are monthly and whose last `nQ` columns are quarterly (observed every 3rd row, `NaN` otherwise), with trailing `NaN`s for the ragged edge. Four estimators are provided: a dynamic factor model estimated by the EM algorithm with Mariano-Murasawa temporal aggregation for large panels, a large Bayesian VAR with data-driven GLP shrinkage hyperparameters for medium panels, bridge equations (pairwise OLS combined by median) as a fast transparent baseline, and single-indicator MIDAS (documented with the MIDAS module). A news decomposition attributes each nowcast revision between data vintages to individual releases using Kalman-gain weights from the DFM state space. The `nowcast()` function extracts the current-quarter estimate plus a one-quarter-ahead forecast from any fitted model.

# Functions

| Function | Signature | Role |
|---|---|---|
| [nowcast_dfm](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/dfm.jl) | `nowcast_dfm(Y, nM, nQ; r=2, p=1, idio=:ar1, blocks=nothing, max_iter=100, thresh=1e-4) -> NowcastDFM` | EM-estimated dynamic factor model with Mariano-Murasawa [1 2 3 2 1] quarterly aggregation |
| [nowcast_bvar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/bvar_nowcast.jl) | `nowcast_bvar(Y, nM, nQ; lags=5, thresh=1e-6, max_iter=nothing, lambda0=0.2, theta0=1.0, miu0=1.0, alpha0=2.0, prior=:conjugate, theta_cross0=nothing) -> NowcastBVAR` | Large BVAR with marginal-likelihood-tuned shrinkage; Kalman smoother fills the ragged edge |
| [nowcast_bridge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/bridge.jl) | `nowcast_bridge(Y, nM, nQ; lagM=1, lagQ=1, lagY=1) -> NowcastBridge` | Pairwise OLS bridge equations on quarterly aggregates, combined by median |
| [nowcast_news](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/news.jl) | `nowcast_news(X_new, X_old, model::NowcastDFM, target_period; target_var=size(X_new,2), groups=nothing, group_names=nothing) -> NowcastNews` | Splits a revision between vintages into per-release news, revision, and re-estimation impacts |
| [nowcast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/forecast.jl) | `nowcast(model::AbstractNowcastModel; target_var=nothing) -> NowcastResult` | Current-quarter estimate plus one-quarter-ahead forecast from any fitted nowcast model |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/forecast.jl) | `forecast(model::NowcastDFM, h; target_var=nothing)` / `forecast(model::NowcastBVAR, h; target_var=nothing)` | Multi-step monthly forecast path; horizon counts months, and no bridge method exists |
| [NowcastDFM](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastDFM{T<:AbstractFloat} <: AbstractNowcastModel` | DFM fit: smoothed panel `X_sm`, factors `F`, state-space matrices, `loglik`, `n_iter` |
| [NowcastBVAR](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastBVAR{T<:AbstractFloat} <: AbstractNowcastModel` | BVAR fit: `beta`, `sigma`, optimal `lambda`/`theta`/`miu`/`alpha`, `converged` flag |
| [NowcastBridge](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastBridge{T<:AbstractFloat} <: AbstractNowcastModel` | Bridge fit: `Y_nowcast` medians, `Y_individual` per-equation predictions, `coefficients` |
| [NowcastResult](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastResult{T<:AbstractFloat}` | Extracted `nowcast`, `forecast`, and `method` (:dfm, :bvar, or :bridge) |
| [NowcastNews](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastNews{T<:AbstractFloat}` | Decomposition: `old_nowcast`, `new_nowcast`, `impact_news`, `impact_revision`, `impact_reestimation`, group impacts |
| [NowcastForecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nowcast/types.jl) | `struct NowcastForecast{T<:AbstractFloat}` | Horizon table from `forecast`; indexes like the underlying array |

Note: `balance_panel` (panel imputation via the DFM, `TimeSeriesData`/`PanelData` dispatch) lives in `src/data/panel.jl` and is documented on the DFM nowcasting docs page.

# Examples

```julia
using MacroEconometricModels
# Y: T x N mixed-frequency panel, first nM columns monthly, last nQ quarterly (NaN off quarter-ends)
dfm = nowcast_dfm(Y, nM, nQ; r=2, p=1, idio=:ar1)
report(dfm)
result = nowcast(dfm)
report(result)
X_old = copy(Y); X_old[end, 1:3] .= NaN  # vintage before three releases
news = nowcast_news(Y, X_old, dfm, size(Y, 1); target_var=nM + nQ)
report(news)
```

# See also

- /forecasting/fceval.md
- /multivariate/factor.md
- /multivariate/bvar.md
- /multivariate/midas.md
- /infrastructure/data.md
