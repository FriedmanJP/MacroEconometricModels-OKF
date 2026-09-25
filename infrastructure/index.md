# Infrastructure

Shared foundations every estimator builds on: the core kernel, data containers, simulators, moment-based estimation, and plotting.

* [Shared Kernel (Core Utilities, Innovation Accounting, Identification)](core.md) - Innovation accounting, SVAR identification, HAC covariance, Kalman kernel, reproducibility, and display utilities shared by every estimator.
* [Data Management (Containers, Transforms, Panels, Cleaning)](data.md) - Typed time-series, panel, and cross-section containers with FRED transforms, validation, panel ops, and direct estimation dispatch.
* [Simulation (Data-Generating Processes)](dgp.md) - NamedTuple-returning simulators with explicit RNGs, burn-in, and population truth for Monte Carlo and recovery checks.
* [Generalized and Simulated Method of Moments (GMM/SMM)](gmm.md) - One-step, optimal, two-step, and iterated GMM plus simulation-based SMM with Hansen J-tests and Andrews-Lu selection.
* [Visualization (Offline D3 Plotting)](plotting.md) - Single-entry-point plotting rendering every result as a self-contained, theme-aware HTML document with vendored D3.js.
