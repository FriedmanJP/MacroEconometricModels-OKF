---
type: Feature
title: ARCH Models
description: Engle ARCH(q) models for time-varying conditional variance with MLE estimation and variance forecasting.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/arch
tags: [arch, volatility, univariate, conditional-variance]
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-volatility
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/volatility.md
    title: Upstream Volatility Models documentation (ARCH section)
  - id: upstream-src-arch
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/arch
    title: Upstream arch source directory
---

# Summary

The `arch` module implements the ARCH(q) model of Engle (1982), the foundation
of the GARCH family: conditional variance is a function of past squared
innovations, `sigma2_t = omega + sum alpha_i * eps2_{t-i}`. Estimation is
two-stage maximum likelihood (Nelder-Mead start, L-BFGS refinement) with
log-transformed parameters enforcing positivity and delta-method standard
errors. Stationarity requires `sum alpha < 1`. The module also defines the
shared `AbstractVolatilityModel` show machinery and `VolatilityForecast`
plotting used by the whole volatility family.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_arch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/estimation.jl) | `estimate_arch(y, q; method=:mle)` | Two-stage MLE of ARCH(q); returns ARCHModel |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/forecast.jl) | `forecast(m, h; conf_level=0.95, n_sim=10000, ...)` | Simulation-based multi-step variance forecasts; returns VolatilityForecast |
| [persistence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/types.jl) | `persistence(m)` | Volatility persistence `sum alpha` |
| [halflife](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/types.jl) | `halflife(m)` | Shock half-life `log(0.5)/log(persistence)`; `Inf` if non-stationary |
| [unconditional_variance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/types.jl) | `unconditional_variance(m)` | Long-run variance `omega / (1 - sum alpha)` |
| [arch_order](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/types.jl) | `arch_order(m)` | ARCH order q |
| [arch_lm_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `arch_lm_test(y, q=5)` or `arch_lm_test(m, q=5)` | Engle ARCH-LM test on raw data or fitted-model residuals |
| [ljung_box_squared](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `ljung_box_squared(z, K=10)` or `ljung_box_squared(m, K=10)` | Ljung-Box test on squared standardized residuals |
| [StatsAPI interface](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/arch/types.jl) | `coef`, `nobs`, `residuals`, `predict`, `loglikelihood`, `aic`, `bic`, `stderror`, `vcov`, `confint` | Standard accessors (`predict` returns the conditional variance series) |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
spx = filter(isfinite, to_vector(apply_tcode(fred[:, ["S&P 500"]])))
arch = estimate_arch(spx, 3)
report(arch)
fc = forecast(arch, 20; conf_level=0.95)
report(fc)
```

# See also

* [GARCH Models](garch.md) - Parsimonious persistence via lagged variances, leverage, and long-memory extensions.
* [Stochastic Volatility](sv.md) - Parameter-driven latent-volatility alternative to ARCH.
* [Spectral Analysis](spectral.md) - Portmanteau serial-correlation tests.
* [MacroEconometricModels.jl overview](../overview.md) - Package feature map.
