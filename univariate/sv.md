---
type: Feature
title: Stochastic Volatility
description: Bayesian stochastic volatility models with latent AR(1) log-variance estimated by Gibbs sampling.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/sv
tags: [stochastic-volatility, bayesian, gibbs, volatility, univariate]
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-volatility
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/volatility.md
    title: Upstream Volatility Models documentation (stochastic volatility section)
  - id: upstream-src-sv
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/sv
    title: Upstream sv source directory
---

# Summary

The `sv` module implements the parameter-driven stochastic volatility model of
Taylor (1986): returns load on a latent AR(1) log-variance with its own
innovation, unlike the observation-driven GARCH family. Estimation is the
Kim-Shephard-Chib (1998) Gibbs sampler with the Omori et al. (2007)
10-component mixture approximation, a simulation smoother for the latent
states, and weakly informative default priors. Variants add a leverage
correlation between return and volatility shocks (`leverage=true`) or
Student-t observation errors (`dist=:studentt`). Forecasts are posterior
predictive simulations from the MCMC draws.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_sv](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/sv/estimation.jl) | `estimate_sv(y; n_samples=2000, burnin=1000, dist=:normal, leverage=false, quantile_levels=[0.025, 0.5, 0.975], seed=nothing, rng=...)` | KSC Gibbs sampler fit; returns SVModel with posterior draws |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/sv/forecast.jl) | `forecast(m, h; conf_level=0.95, rng=...)` | Posterior predictive volatility forecast; returns VolatilityForecast |
| [persistence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/sv/types.jl) | `persistence(m)` | Posterior mean of the log-volatility persistence `phi` |
| [unconditional_variance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/sv/types.jl) | `unconditional_variance(m)` | Long-run variance `exp(E[mu])` |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
spx = filter(isfinite, to_vector(apply_tcode(fred[:, ["S&P 500"]])))
sv = estimate_sv(spx; n_samples=2000, burnin=1000)
report(sv)
fc = forecast(sv, 20; conf_level=0.95)
report(fc)
```

# See also

* [ARCH Models](arch.md) - Observation-driven conditional variance foundation.
* [GARCH Models](garch.md) - EGARCH and GJR-GARCH leverage counterparts to SV leverage.
* [MacroEconometricModels.jl overview](../overview.md) - Package feature map.
