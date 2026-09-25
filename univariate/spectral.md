---
type: Feature
title: Spectral Analysis
description: ACF/PACF, spectral density, cross-spectrum, frequency-domain filtering, and white-noise diagnostics.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/spectral
tags: [spectral, acf, periodogram, coherence, filtering, diagnostics, univariate]
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/univariate
  at: 2026-09-25T01:06:33Z
sources:
  - id: upstream-docs-spectral
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/spectral.md
    title: Upstream Spectral Analysis documentation
  - id: upstream-src-spectral
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/spectral
    title: Upstream spectral source directory
---

# Summary

The `spectral` module is a time- and frequency-domain toolkit: sample ACF,
PACF (Levinson-Durbin or OLS), and CCF with cumulative Ljung-Box Q-statistics;
the raw periodogram plus three consistent spectral density estimators (Welch,
Daniell-smoothed, Burg AR parametric); Welch-based cross-spectra with
coherence, phase, and gain; the ideal bandpass filter and analytical transfer
functions for the HP, Baxter-King, and Hamilton filters; and white-noise
diagnostics (Fisher hidden-periodicity, Bartlett cumulative periodogram, and
the Ljung-Box, Box-Pierce, and Durbin-Watson portmanteau tests, which live in
`src/teststat/`). All frequencies are radians per observation on `[0, pi]`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [acf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/acf.jl) | `acf(y; lags=0, conf_level=0.95)` | Sample ACF with Ljung-Box Q-stats; returns ACFResult |
| [pacf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/acf.jl) | `pacf(y; lags=0, method=:levinson, conf_level=0.95)` | Partial ACF via Levinson-Durbin or OLS |
| [acf_pacf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/acf.jl) | `acf_pacf(y; lags=0, method=:levinson, conf_level=0.95)` | Joint correlogram in a single pass |
| [ccf](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/acf.jl) | `ccf(x, y; lags=0, conf_level=0.95)` | Cross-correlation on lags `-k:k` |
| [periodogram](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/estimation.jl) | `periodogram(y; window=:rectangular, conf_level=0.95)` | Raw FFT periodogram (inconsistent by design) |
| [spectral_density](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/estimation.jl) | `spectral_density(y; method=:welch, kwargs...)` | Consistent estimators: `:welch`, `:smoothed`, `:ar` |
| [cross_spectrum](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/cross.jl) | `cross_spectrum(x, y; window=:hann, segment_length=0, overlap=0.5)` | Welch cross-spectrum; returns CrossSpectrumResult |
| [coherence](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/cross.jl) | `coherence(x, y; kwargs...)` | Convenience accessor returning `(freq, squared-coherence)` |
| [phase](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/cross.jl) | `phase(x, y; kwargs...)` | Convenience accessor returning `(freq, lead-lag-phase)` |
| [gain](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/cross.jl) | `gain(x, y; kwargs...)` | Convenience accessor returning `(freq, amplitude-ratio)` |
| [band_power](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/diagnostics.jl) | `band_power(result, f_low, f_high)` | Trapezoidal variance share of a spectral density over a band |
| [ideal_bandpass](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/filtering.jl) | `ideal_bandpass(y, f_low, f_high)` | Sharp-cutoff frequency filter (Gibbs ringing warning applies) |
| [transfer_function](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/filtering.jl) | `transfer_function(filter; lambda=1600, K=12, h=8, n_freq=256)` | Gain/phase of `:hp`, `:bk`, or `:hamilton`; returns TransferFunctionResult |
| [fisher_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/diagnostics.jl) | `fisher_test(y)` | Fisher exact test for a hidden periodicity |
| [bartlett_white_noise_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/spectral/diagnostics.jl) | `bartlett_white_noise_test(y)` | KS test of the cumulative normalized periodogram |
| [ljung_box_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/portmanteau.jl) | `ljung_box_test(y; lags=0, fitdf=0)` | Portmanteau autocorrelation test (defined in teststat) |
| [box_pierce_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/portmanteau.jl) | `box_pierce_test(y; lags=0, fitdf=0)` | Original Box-Pierce Q test (defined in teststat) |
| [durbin_watson_test](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/teststat/portmanteau.jl) | `durbin_watson_test(resid)` | First-order autocorrelation test (defined in teststat) |

# Examples

```julia
using MacroEconometricModels
fred = load_example(:fred_md)
y = filter(isfinite, to_vector(apply_tcode(fred[:, ["INDPRO"]])))[end-99:end]
result = acf_pacf(y; lags=24)
show(stdout, result)
sd = spectral_density(y; method=:welch)
show(stdout, sd)
```

# See also

* [ARIMA Models](arima.md) - The correlogram feeds ARMA order selection.
* [Time Series Filters](filters.md) - Filters whose frequency responses `transfer_function` evaluates.
* [ARCH Models](arch.md) - ARCH-LM and squared-residual diagnostics for fitted volatility models.
* [MacroEconometricModels.jl overview](../overview.md) - Package feature map.
