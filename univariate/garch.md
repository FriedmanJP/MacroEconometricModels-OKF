---
type: Feature
title: GARCH Models
description: GARCH, EGARCH, GJR-GARCH, GARCH-MIDAS, FIGARCH, IGARCH, component GARCH, and APARCH volatility models.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/garch
tags: [garch, egarch, leverage, volatility, univariate, long-memory, midas]
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-volatility
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/volatility.md
    title: Upstream Volatility Models documentation (GARCH sections)
  - id: upstream-src-garch
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/garch
    title: Upstream garch source directory
---

# Summary

The `garch` module covers the observation-driven volatility family: standard
GARCH(p,q), EGARCH and GJR-GARCH for leverage effects, GARCH-MIDAS for
mixed-frequency long-run variance, FIGARCH/FIEGARCH for long-memory
persistence, plus IGARCH, component GARCH, and APARCH. GARCH, EGARCH, and
GJR-GARCH support Gaussian, standardized Student-t, and GED conditional
distributions via `dist`, and report Bollerslev-Wooldridge QMLE-robust
standard errors by default. Shared tools include news impact curves,
simulation-based multi-step forecasts, and the Engle-Ng sign-bias and
Nyblom-Hansen stability tests.

# Functions

| Function | Signature | Role |
|---|---|---|
| [estimate_garch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_garch(y, p=1, q=1; method=:mle, dist=:normal)` | GARCH(p,q); `dist` selects `:normal`, `:student`, or `:ged` |
| [estimate_egarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_egarch(y, p=1, q=1; method=:mle, dist=:normal)` | Log-variance EGARCH with leverage; no positivity constraints |
| [estimate_gjr_garch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_gjr_garch(y, p=1, q=1; method=:mle, dist=:normal)` | Threshold GARCH with indicator leverage term |
| [estimate_garch_midas](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/midas.jl) | `estimate_garch_midas(r, x_lf; K=12, m_freq, rv=:macro, span=:fixed)` | MIDAS with exogenous low-frequency driver; returns GarchMidasModel |
| [estimate_garch_midas](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/midas.jl) | `estimate_garch_midas(r; K=12, m_freq, rv=:realized, span=:fixed)` | MIDAS with block realized variance as the long-run driver |
| [estimate_figarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/figarch.jl) | `estimate_figarch(r; p=1, q=1, d0=0.4, truncation=1000, dist=:normal)` | Fractionally integrated GARCH; truncated ARCH(inf) recursion |
| [estimate_fiegarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/figarch.jl) | `estimate_fiegarch(r; p=1, q=1, d0=0.4, truncation=1000, dist=:normal)` | Log-variance FIEGARCH with asymmetric news function |
| [estimate_igarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_igarch(y, p=1, q=1; method=:mle)` | Unit-persistence IGARCH; RiskMetrics EWMA is the `omega=0` case |
| [estimate_cgarch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_cgarch(y; method=:mle)` | Engle-Lee permanent/transitory component GARCH(1,1) |
| [estimate_aparch](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/estimation.jl) | `estimate_aparch(y, p=1, q=1; fix_delta=nothing, fix_gamma=nothing, method=:mle)` | Asymmetric power ARCH; nests GARCH, GJR, and TARCH |
| [forecast](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/forecast.jl) | `forecast(m, h; conf_level=0.95, n_sim=10000, ...)` | Simulation-based variance forecasts for every GARCH-family model |
| [news_impact_curve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/diagnostics.jl) | `news_impact_curve(m; range=(-3.0, 3.0), n_points=200)` | Shock-to-variance map; returns `(shocks, variance)` |
| [component_variances](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `component_variances(m)` | CGARCH permanent, transitory, and total variance series |
| [sign_bias_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/diagnostics.jl) | `sign_bias_test(z_or_model)` | Engle-Ng joint sign/size-bias test for missed asymmetry |
| [nyblom_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/diagnostics.jl) | `nyblom_test(m)` | Nyblom-Hansen parameter-stability test with Hansen critical values |
| [persistence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `persistence(m)` | Model-specific persistence (sums of alpha/beta/gamma, or d) |
| [halflife](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `halflife(m)` | Shock half-life; `Inf` when persistence is 1 |
| [unconditional_variance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `unconditional_variance(m)` | Model-specific long-run variance |
| [arch_order](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `arch_order(m)` | ARCH order q |
| [garch_order](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/garch/types.jl) | `garch_order(m)` | GARCH order p |
| [arch_lm_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `arch_lm_test(y, q=5)` or `arch_lm_test(m, q=5)` | Engle ARCH-LM test on raw data or fitted-model residuals |
| [ljung_box_squared](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/arch_diagnostics.jl) | `ljung_box_squared(z, K=10)` or `ljung_box_squared(m, K=10)` | Ljung-Box test on squared standardized residuals |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
spx = filter(isfinite, to_vector(apply_tcode(fred[:, ["S&P 500"]])))
garch = estimate_garch(spx, 1, 1)
report(garch)
(persistence = persistence(garch), halflife = halflife(garch))
```

# See also

* [ARCH Models](/univariate/arch.md) - The single-lag foundation of the GARCH family.
* [Stochastic Volatility](/univariate/sv.md) - Latent-volatility alternative with leverage and Student-t errors.
* [Multivariate GARCH](/multivariate/) - CCC, DCC, and BEKK covariance models reusing these margins.
* [MacroEconometricModels.jl overview](/overview.md) - Package feature map.
