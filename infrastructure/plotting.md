---
type: Feature
title: Visualization (Offline D3 Plotting)
description: Single-entry-point plotting rendering every result as a self-contained, theme-aware HTML document with vendored D3.js.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/13e3522c/src/plotting
tags:
  - infrastructure
  - plotting
  - visualization
  - d3
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/infrastructure
  at: 2026-09-25T01:38:10Z
sources:
  - id: plotting-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/docs/src/plotting.md
    title: Visualization docs page
  - id: src-plotting
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/13e3522c/src/plotting
    title: src/plotting module source
---

# Summary

The `plotting` module renders every plottable result through one entry point, `plot_result`, returning a `PlotOutput` that wraps a self-contained HTML document: D3.js v7 is vendored offline and inlined, so plots render identically air-gapped, and documents are theme-aware (light/dark). The exported surface is exactly `plot_result`, `PlotOutput`, `save_plot`, and `display_plot`; all other plotting names are internal. A standardized keyword vocabulary (`var`, `shock`, `vars`, `view`, `ncols`, `title`, `save_path`, `history`/`n_history`, `stat`, plus plot-tuning keywords `conf_level`, `level`, `log_scale`, `lags`, `threshold`, `bw`/`n_grid`) carries identical meaning across dispatches, multi-view results switch exhibits with `view=`, and name-or-index selection raises `ArgumentError` on unknown names. Coverage spans unit-root, panel, cointegration, portmanteau, causality, and specification test plots, count/quantile/robust/panel/ARDL and DiD model plots, and a `:stacked` exhibit for Bayesian historical decompositions; spectral-density plots default to a linear scale (`log_scale=true` restores log10). The table shows the exported surface plus representative dispatches; the full dispatch catalog (230 methods across 42 source files) is generated at docs build time from `methods(plot_result)`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/irf.jl) | `plot_result(r::ImpulseResponse; var=nothing, shock=nothing, ncols=0, title="", save_path=nothing)` | IRF panels with confidence bands and zero reference |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/irf.jl) | `plot_result(r::BayesianImpulseResponse; var=nothing, shock=nothing, stat=:median, draws=0, ...)` | Bayesian IRF as nested posterior credible fans |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/irf.jl) | `plot_result(r::LPImpulseResponse; var=nothing, ncols=0, title="", save_path=nothing)` | LP IRFs with robust confidence bands |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/timeseries.jl) | `plot_result(d::TimeSeriesData; view=:line, vars=nothing, ...)` | Container views: line, scatter, hist, density, corr, growth, binscatter |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/teststat.jl) | `plot_result(r::ADFResult; title="", save_path=nothing, level=5)` plus unit-root, panel, cointegration, portmanteau, causality, and specification siblings | Statistic-vs-critical-value bars at the `level`% critical value |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/hd.jl) | `plot_result(hd::BayesianHistoricalDecomposition; stat=:mean, view=:fan, ...)` | Per-(variable, shock) credible fans, or `view=:stacked` central-estimate bars plus actual-vs-reconstructed |
| [PlotOutput](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/types.jl) | `struct PlotOutput` | Self-contained HTML document plus embeddable fragment |
| [save_plot](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/render.jl) | `save_plot(p::PlotOutput, path::String)` | Write the HTML visualization to a file |
| [display_plot](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/13e3522c/src/plotting/render.jl) | `display_plot(p::PlotOutput)` | Open the visualization in the default browser |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
Y = to_matrix(apply_tcode(fred[:, ["INDPRO", "CPIAUCSL", "FEDFUNDS"]]))
Y = Y[all.(isfinite, eachrow(Y)), :]
m = estimate_var(Y, 4)
r = irf(m, 20; ci_type=:bootstrap, reps=500)
p = plot_result(r)
save_plot(p, "irf_plot.html")
```

# See also

* [Data Management](data.md) - containers and the `:corr`/`:growth` views over them
* [Shared Kernel](core.md) - IRF/FEVD/HD results that feed the plots
* [Simulation (DGPs)](dgp.md) - synthetic samples for gallery-style exhibits
* [Forecast Evaluation](../forecasting/fceval.md) - metric and Theil views of forecast comparisons
* [Vector Autoregression](../multivariate/var.md) - the canonical plotted model
