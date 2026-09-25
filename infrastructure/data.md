---
type: Feature
title: Data Management (Containers, Transforms, Panels, Cleaning)
description: Typed time-series, panel, and cross-section containers with FRED transforms, validation, panel ops, and direct estimation dispatch.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/data
tags:
  - infrastructure
  - data
  - timeseries
  - panel
  - fred
  - transforms
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/infrastructure
  at: 2026-09-25T01:38:10Z
sources:
  - id: data-page
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/data.md
    title: Data Management docs page
  - id: src-data
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/data
    title: src/data module source
---

# Summary

The `data` module provides the typed containers every estimator accepts: `TimeSeriesData`, `PanelData`, and `CrossSectionData`, each carrying variable names, descriptions, and bibliographic references alongside the numbers. Thirteen built-in datasets (FRED-MD/QD, PWT, DDCG, mpdta, Grunfeld, Mroz, stackloss, Nile, Hamilton GNP, Denmark, the `:wiot` two-sector Miller & Blair toy table, monetary-policy shocks) load via `load_example`; FRED transformation codes 1-7 map levels to stationary series with `inverse_tcode` reconstructing levels; `diagnose`/`fix`/`dropna`/`keeprows` validate and clean; Stata-style `xtset` plus within-group lag/lead/difference and DFM gap-filling cover panels; and container methods on 40+ estimators mean no manual `to_matrix` calls.

# Functions

| Function | Signature | Role |
|---|---|---|
| [TimeSeriesData](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `TimeSeriesData(data::AbstractMatrix; varnames, frequency=Other, tcode, time_index, ...)` | Time-series container with metadata |
| [PanelData](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `struct PanelData{T} <: AbstractMacroData` | Stacked panel container with group/time ids |
| [CrossSectionData](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `CrossSectionData(data::AbstractMatrix; varnames, obs_id, ...)` | Cross-section container with observation ids |
| [Frequency](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `@enum Frequency Daily Monthly Quarterly Yearly Mixed Other` | Data-frequency metadata enum |
| [nobs](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `StatsAPI.nobs(d::AbstractMacroData)` | Number of observations |
| [nvars](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `nvars(d::AbstractMacroData)` | Number of variables |
| [varnames](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `varnames(d::AbstractMacroData)` | Variable names |
| [frequency](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `frequency(d::TimeSeriesData)` | Data frequency metadata |
| [time_index](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `time_index(d::TimeSeriesData)` | Integer time identifiers |
| [dates](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `dates(d::TimeSeriesData)` | Date labels for indexing and plot axes |
| [obs_id](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `obs_id(d::CrossSectionData)` | Observation identifiers |
| [desc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `desc(d::AbstractMacroData)` | Dataset description |
| [vardesc](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `vardesc(d::AbstractMacroData, name::String)` | Per-variable description |
| [rename_vars!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `rename_vars!(d, old::String => new::String)` | Rename variables in place, descriptions follow |
| [set_dates!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `set_dates!(d::TimeSeriesData, dt::Vector{String})` | Attach date labels, enabling date indexing |
| [set_desc!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `set_desc!(d::AbstractMacroData, text::String)` | Set the dataset description |
| [set_vardesc!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/types.jl) | `set_vardesc!(d::AbstractMacroData, name::String, text::String)` | Set one variable description |
| [load_example](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/examples.jl) | `load_example(name::Symbol)` | Load a built-in dataset container |
| [apply_tcode](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/transform.jl) | `apply_tcode(y::AbstractVector, tcode::Int)` | FRED transform of a raw vector |
| [apply_tcode](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/transform.jl) | `apply_tcode(d::TimeSeriesData, tcodes::Vector{Int})` | Per-variable FRED transforms, rows aligned |
| [inverse_tcode](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/transform.jl) | `inverse_tcode(y, tcode::Int; x_prev=nothing)` | Reconstruct levels from a transformed series |
| [DataDiagnostic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `struct DataDiagnostic` | NaN/Inf/constant/short-sample diagnostic |
| [diagnose](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `diagnose(d::AbstractMacroData)` | Scan a container for data problems |
| [fix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `fix(d::TimeSeriesData; method=:listwise)` | Clean copy via listwise, interpolate, or mean |
| [dropna](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `dropna(d::TimeSeriesData; vars=nothing)` | Drop rows holding NaN or Inf |
| [keeprows](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `keeprows(d::TimeSeriesData, idx::Vector{Int})` | Subset rows keeping metadata aligned |
| [validate_for_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/validation.jl) | `validate_for_model(d::AbstractMacroData, model_type::Symbol)` | Check dimensionality a model family needs |
| [xtset](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `xtset(df::DataFrame, group_col::Symbol, time_col::Symbol; cohort=nothing, ...)` | Build a PanelData Stata-style |
| [isbalanced](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `isbalanced(d::PanelData)` | Whether every group has equal rows |
| [groups](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `groups(d::PanelData)` | Group labels |
| [ngroups](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `ngroups(d::PanelData)` | Number of groups |
| [group_data](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `group_data(d::PanelData, g::String)` | Extract one group as TimeSeriesData |
| [panel_summary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `panel_summary(d::PanelData)` | Print panel structure summary |
| [balance_panel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `balance_panel(pd::PanelData; method=:dfm, r=3, p=2)` | Fill NaN via DFM Kalman smoothing |
| [panel_lag](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `panel_lag(pd::PanelData, var, k::Int=1)` | Within-group lag respecting gaps |
| [panel_lead](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `panel_lead(pd::PanelData, var, k::Int=1)` | Within-group lead respecting gaps |
| [panel_diff](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `panel_diff(pd::PanelData, var)` | Within-group first difference |
| [add_panel_lag](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `add_panel_lag(pd::PanelData, var, k::Int=1)` | Append a `lag{k}_{var}` column |
| [add_panel_lead](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `add_panel_lead(pd::PanelData, var, k::Int=1)` | Append a `lead{k}_{var}` column |
| [add_panel_diff](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/panel.jl) | `add_panel_diff(pd::PanelData, var)` | Append a `d_{var}` column |
| [describe_data](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/summary_stats.jl) | `describe_data(d::AbstractMacroData)` | Per-variable summary statistics table |
| [DataSummary](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/summary_stats.jl) | `struct DataSummary` | Moments, quartiles, skew, kurtosis per variable |
| [to_matrix](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/convert.jl) | `to_matrix(d::TimeSeriesData)` | Raw data matrix |
| [to_vector](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/convert.jl) | `to_vector(d::TimeSeriesData, var::String)` | One column by name |
| [estimate_var](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/convert.jl) | `estimate_var(d::TimeSeriesData, p::Int; kwargs...)` | Container dispatch; 40+ estimators share the pattern |
| [apply_filter](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/filter.jl) | `apply_filter(d::TimeSeriesData, specs::AbstractVector; component=:cycle, ...)` | Per-variable trend-cycle filter |
| [apply_filter](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/data/filter.jl) | `apply_filter(d::TimeSeriesData, spec::Union{Symbol, AbstractFilterResult}; vars=nothing, ...)` | One filter over all or selected variables |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
sub = fred[:, ["INDPRO", "CPIAUCSL", "FEDFUNDS"]]
d = apply_tcode(sub)
d = fix(d)
describe_data(d)
model = estimate_var(d, 2)
report(model)
```

# See also

* [Shared Kernel](core.md) - reproducibility, persistence, and tabular exports for containers
* [Simulation (DGPs)](dgp.md) - synthetic samples for Monte Carlo checks
* [Visualization](plotting.md) - container views (`:line`, `:corr`, panel views)
* [Vector Autoregression](../multivariate/var.md) - estimation straight from a container
* [Difference-in-Differences](../panel/did.md) - consumer of panel cohorts
* [Input-Output Analysis](../io/io.md) - home of the `:wiot` example dataset
