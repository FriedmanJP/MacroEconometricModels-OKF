---
okf_version: "0.2"
---

# MacroEconometricModels.jl — OKF knowledge bundle

An [OKF v0.2](https://github.com/GoogleCloudPlatform/open-knowledge-format)
bundle describing the feature areas of
[MacroEconometricModels.jl](https://github.com/FriedmanJP/MacroEconometricModels.jl).
Start at the package overview, then drill into a domain.

## Package overview

* [MacroEconometricModels.jl overview](overview.md) - What the package is, its feature map, and how to install it.

## Domains

* [Univariate](univariate/index.md) - ARIMA, ARCH/GARCH, stochastic volatility, filters, X-13 seasonal adjustment, and spectral analysis.
* [Nonlinear & state-space](nonlinear-statespace/index.md) - Threshold, STAR, and Markov-switching models plus linear-Gaussian state-space methods.
* [Multivariate](multivariate/index.md) - VAR, VECM, Bayesian VAR, local projections, factor models, and multivariate GARCH.
* [Panel](panel/index.md) - Panel VAR, panel regression and IV, panel ARDL, difference-in-differences, and event studies.
* [DSGE](dsge/index.md) - DSGE specification and solvers, heterogeneous-agent and continuous-time models, and OLG.
* [Input-output](io/index.md) - Input-output tables, multipliers, linkages, structural decomposition, and production networks.
* [Policy counterfactuals](policy-counterfactuals/index.md) - Sufficient-statistics policy analysis: rule counterfactuals, OPP, and model banks.
* [Cross-section](cross-section/index.md) - OLS/IV, penalized and robust regression, limited dependent variables, and discrete choice.
* [Nonparametric](nonparametric/index.md) - Kernel density estimation, kernel and local-polynomial regression, and LOWESS.
* [Forecasting](forecasting/index.md) - Multi-model forecasting, forecast evaluation and combination, and nowcasting.
* [Testing](testing/index.md) - Unit-root, cointegration, break, and general specification and hypothesis tests.
* [Infrastructure](infrastructure/index.md) - Core utilities, data handling, DGP simulation, GMM, and plotting.

## License

This bundle is licensed under the [Apache License, Version 2.0](LICENSE),
matching the upstream [open-knowledge-format](https://github.com/GoogleCloudPlatform/open-knowledge-format) project.
