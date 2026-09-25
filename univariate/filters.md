---
type: Feature
title: Time Series Filters
description: HP, Hamilton, Beveridge-Nelson, Baxter-King, and boosted HP trend-cycle decompositions.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/filters
tags: [filters, hp-filter, trend-cycle, business-cycle, univariate]
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-filters
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/filters.md
    title: Upstream Time Series Filters documentation
  - id: upstream-src-filters
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/filters
    title: Upstream filters source directory
---

# Summary

The `filters` module provides five trend-cycle decompositions: the
Hodrick-Prescott penalized smoother with frequency-matched lambda, the
Hamilton (2018) predictive-regression filter, the Beveridge-Nelson
permanent-transitory decomposition (ARIMA or correlated unobserved-components
state-space route), the Baxter-King band-pass filter, and the iterated boosted
HP filter with BIC, ADF, or fixed stopping. Every result supports the unified
`trend()` and `cycle()` accessors plus `report()` and `plot_result()`, so
consuming code never needs to know which filter produced a decomposition.

# Functions

| Function | Signature | Role |
|---|---|---|
| [hp_filter](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/hp.jl) | `hp_filter(y; lambda=1600)` | Penalized-least-squares smoother; returns HPFilterResult |
| [hamilton_filter](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/hamilton.jl) | `hamilton_filter(y; h=8, p=4)` | OLS projection filter; loses h+p-1 obs; returns HamiltonFilterResult |
| [beveridge_nelson](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/beveridge_nelson.jl) | `beveridge_nelson(y; method=:arima, p=:auto, q=:auto, max_terms=500, cycle_order=2)` | Permanent-transitory split; returns BeveridgeNelsonResult |
| [baxter_king](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/baxter_king.jl) | `baxter_king(y; pl=6, pu=32, K=12)` | Symmetric band-pass filter; loses K obs per end; returns BaxterKingResult |
| [boosted_hp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/boosted_hp.jl) | `boosted_hp(y; lambda=1600, stopping=:BIC, max_iter=100, sig_p=0.05)` | Iterated HP with data-driven stopping; returns BoostedHPResult |
| [trend](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/types.jl) | `trend(r)` | Unified trend accessor (permanent component for Beveridge-Nelson) |
| [cycle](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/filters/types.jl) | `cycle(r)` | Unified cycle accessor (transitory component for Beveridge-Nelson) |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
y = filter(isfinite, log.(fred[:, "INDPRO"]))
hp = hp_filter(y; lambda=129600.0)
report(hp)
std(cycle(hp))
```

# See also

* [X-13ARIMA-SEATS](/univariate/x13.md) - Removes the seasonal component rather than the trend.
* [Spectral Analysis](/univariate/spectral.md) - `transfer_function` plots each filter's frequency response.
* [ARIMA Models](/univariate/arima.md) - Beveridge-Nelson selects its ARMA order via `auto_arima`.
* [MacroEconometricModels.jl overview](/overview.md) - Package feature map.
