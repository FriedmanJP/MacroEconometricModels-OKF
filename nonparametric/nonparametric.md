---
type: Feature
title: Nonparametric Regression and Density
description: Kernel density estimation, Nadaraya-Watson and local-polynomial regression with cross-validated bandwidths, and robust LOWESS smoothing.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nonparametric
tags:
  - nonparametric
  - kernel
  - density
  - regression
  - lowess
  - smoothing
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/small-domains
  at: 2026-09-25T01:25:44Z
sources:
  - id: nonparametric-docs
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/nonparametric.md
    title: Nonparametric Regression and Density docs
  - id: src-nonparametric
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/nonparametric
    title: src/nonparametric module source
---

# Summary

The `nonparametric` module estimates distributions and conditional means without imposing a parametric form.
`kernel_density` reconstructs a sample's shape with Gaussian, Epanechnikov, triangular, or uniform kernels and Silverman (`bw.nrd0`) or Sheather-Jones plug-in (`bw.SJ`) bandwidths; `kernel_reg` fits the Nadaraya-Watson local-constant, Fan-Gijbels local-linear (with automatic boundary-bias correction), or arbitrary-degree local-polynomial estimator with leave-one-out cross-validated bandwidths and sandwich pointwise standard-error bands; `lowess` is Cleveland's robust tricube-weighted scatterplot smoother with bisquare robustifying passes, ported to match R's `stats::lowess` to machine precision.
Results return `KernelDensity`, `KernelRegression`, and `LowessFit` objects integrating with `report`, `refs`, and `plot_result`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [kernel_density](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/density.jl) | `kernel_density(y; kernel=:gaussian, bw=:silverman, npoints=512, cut=3.0)` | Kernel density estimate with rule-of-thumb, plug-in, or user bandwidth |
| [kernel_reg](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/kernel_reg.jl) | `kernel_reg(y, x; method=:ll, degree=1, bw=:cv, kernel=:gaussian)` | Local-constant, local-linear, or local-polynomial regression with CV bandwidth and SE bands |
| [lowess](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/lowess.jl) | `lowess(y, x; f=2//3, iter=3, delta=nothing)` | Robust LOWESS smoother with span `f` and bisquare robustifying passes |
| [KernelDensity](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/types.jl) | `struct KernelDensity{T}` | Density grid and `density` vector, bandwidth, kernel, `bw_method`, data, and observation count |
| [KernelRegression](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/types.jl) | `struct KernelRegression{T}` | Sorted fit, standard errors, bandwidth, method, residual variance |
| [LowessFit](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/nonparametric/types.jl) | `struct LowessFit{T}` | Sorted smoothed values, span, iterations, and observation count |

# Examples

```julia
using MacroEconometricModels
kd = kernel_density(z)                        # Gaussian kernel, Silverman bandwidth
kr = kernel_reg(y, x; method=:ll, bw=:cv)     # local-linear, CV bandwidth
lf = lowess(y, x; f=0.3, iter=3)              # robust LOWESS smoother
report(kr)
```

# See also

* [Regression (OLS, IV, and Limited Dependent Variables)](/cross-section/reg.md) - parametric conditional means
* [Nonlinear Time Series](/nonlinear-statespace/nonlinear.md) - parametric transition-function time-series models
