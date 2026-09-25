# Multivariate

Multi-series time-series models: vector autoregressions and their Bayesian
and cointegrated variants, direct impulse-response estimation, factor
structures, mixed-frequency regression, conditional covariances, and
data-driven structural identification.

* [Vector Autoregression (VAR)](var.md) - OLS estimation, lag selection, forecasting, and conditional forecasting for reduced-form VAR(p) models.
* [Vector Error Correction Models (VECM)](vecm.md) - Johansen and Engle-Granger estimation, rank selection, restriction testing, and forecasting for cointegrated systems.
* [Bayesian VAR (BVAR)](bvar.md) - Minnesota-prior Bayesian VAR estimation with marginal-likelihood hyperparameter selection, posterior sampling, and TVP/MF extensions.
* [Local Projections (LP)](lp.md) - Horizon-by-horizon impulse-response estimation with IV, smooth, state-dependent, and propensity-score variants plus LP-FEVD and forecasting.
* [Factor Models](factor.md) - Static, dynamic, generalized dynamic, and structural factor models for compressing large macro panels into latent factors.
* [Factor-Augmented VAR (FAVAR)](favar.md) - Two-step and Bayesian factor-augmented VARs that compress a large panel into latent factors, estimate a VAR on factors plus key observables, and map structural results back to the full panel.
* [Multivariate GARCH (CCC / DCC / BEKK)](mgarch.md) - Conditional covariance modelling for return panels via constant conditional correlation, dynamic conditional correlation, and scalar/diagonal BEKK estimators.
* [Cointegrating Regression (FMOLS / CCR / DOLS)](cointreg.md) - Single-equation estimation of a cointegrating vector by fully-modified OLS, canonical cointegrating regression, and dynamic OLS, plus group-mean and pooled panel extensions.
* [MIDAS Regression](midas.md) - Mixed-data-sampling regression of a low-frequency target on high-frequency lags through exponential-Almon, Beta, or polynomial weights, with ADL-MIDAS, U-MIDAS, and direct forecasting.
* [Statistical Identification (Non-Gaussian and Heteroskedastic SVAR)](nongaussian.md) - Data-driven SVAR identification from non-Gaussian shocks or time-varying volatility without recursive, sign, or exclusion restrictions, with diagnostics and shock labelling.
