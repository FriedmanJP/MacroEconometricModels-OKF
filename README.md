# MacroEconometricModels-OKF

An [Open Knowledge Format (OKF v0.2)](https://github.com/GoogleCloudPlatform/open-knowledge-format)
knowledge bundle describing every feature and function of
[MacroEconometricModels.jl](https://github.com/FriedmanJP/MacroEconometricModels.jl).

## Status

Scaffolding. The authoring plan lives at
[.agents/plans/2026-09-25-memecon-okf-bundle.md](.agents/plans/2026-09-25-memecon-okf-bundle.md);
no OKF concepts have been written yet.

## Layout (planned)

This repository will itself be the OKF bundle (bundle at repo root):

```text
index.md                  # bundle root index (carries okf_version)
log.md                    # bundle update history
overview.md               # type: Package — what MacroEconometricModels.jl is
univariate/               # arima, arch, garch, sv, filters, x13, spectral
nonlinear-statespace/     # nonlinear, statespace
multivariate/             # var, vecm, bvar, lp, factor, favar, mgarch, ...
panel/                    # pvar, preg, did, ardl
dsge/                     # dsge, ct, olg
io/                       # io
policy-counterfactuals/   # counterfactual
cross-section/            # reg, system
nonparametric/            # nonparametric
forecasting/              # fceval, nowcast
testing/                  # teststat
infrastructure/           # core, data, dgp, gmm, plotting
guides/                   # workflow-oriented how-tos
references/               # mirrored run instructions / code (OKF convention)
```

Each domain directory gets an `index.md`; each feature area gets one
`type: Feature` concept with a function table in the body.

## Sources of truth

- OKF v0.2 specification: <https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md>
- Upstream package: <https://github.com/FriedmanJP/MacroEconometricModels.jl> (docs + docstrings)

## License

GNU General Public License v3.0 — see [LICENSE](LICENSE) (mirrors upstream).
