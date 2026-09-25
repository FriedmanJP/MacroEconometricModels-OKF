---
type: Reference
title: Bundle update log
description: Chronological history of changes to this OKF bundle.
---

# Update log

## 2026-09-25

- Bundle initialized at the repository root as an OKF v0.2 bundle
  (`okf_version: "0.2"` in the root `index.md`).
- Added the skeleton: root `index.md`, `overview.md` (`type: Package`),
  and this `log.md`.
- Added conformance tooling: `scripts/validate_okf.py` and
  `.github/workflows/okf-validate.yml` (checks frontmatter, reserved-file
  structure, bundle-relative links, and log date headings on push and
  pull request).
- Upstream provenance pinned to `MacroEconometricModels.jl` commit
  `3d12bb6c`; domain `index.md` stubs and feature concepts follow in the
  pilot and full-coverage passes.
- Full-coverage pass complete: 39 `type: Feature` concepts across 12
  domain directories (`univariate`, `nonlinear-statespace`,
  `multivariate`, `panel`, `dsge`, `io`, `nonparametric`,
  `policy-counterfactuals`, `cross-section`, `forecasting`, `testing`,
  `infrastructure`), each grounded in upstream `docs/src` and `src`
  docstrings at `3d12bb6c`, all `status: draft` pending human review.
- Fixed four bundle-relative cross-links (`panel/preg.md`,
  `panel/pvar.md`, `testing/teststat.md`); `scripts/validate_okf.py`
  passes clean (54 files).
