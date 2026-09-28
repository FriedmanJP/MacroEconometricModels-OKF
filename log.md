---
type: Reference
title: Bundle update log
description: Chronological history of changes to this OKF bundle.
---

# Update log

## 2026-09-28

- Released bundle v1.0.0 (tag `v1.0.0` at `c380c3e`): 39 approved
  `type: Feature` concepts pinned to upstream `3d12bb6c`, whose tree is
  identical to `MacroEconometricModels.jl` v1.0.0.
- Synced upstream v1.0.1 (plot-option and coverage release, closes
  `#836`-`#843`, `#845`-`#849`) and re-pinned all provenance links to
  `13e3522c` for bundle v1.0.1: `forecast(::NowcastBridge)` flat-path
  method (`forecasting/nowcast.md`, replacing the "no bridge method
  exists" note), new `plot_result` keywords (`conf_level`, `level`,
  `log_scale`, `lags`, `threshold`, `bw`/`n_grid`), 52 new dispatches
  (178 to 230 methods), the Bayesian HD `:stacked` view, and the
  spectral linear-scale default (`infrastructure/plotting.md`), plus
  the typed `ct_kfe` `SingularSystemError`
  (`dsge/ct.md`). The v1.0.1 docs-only fixes (thirteen datasets,
  StatsAPI field note, `r_bounds` docstring) were already reflected in
  this bundle and needed no change. The three touched concepts keep
  their 2026-09-25 human verification; re-review of the v1.0.1 deltas
  approved the same day (`verified` re-stamped on the three concepts).

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
- Human review sign-off (Q1): `chung9207` approved all 39 Feature
  concepts and the package overview; `status` flipped from `draft` to
  `approved` with `reviewed_by`/`reviewed_at` recorded.
- License decision (Q2): bundle licensed under Apache License 2.0
  (`LICENSE`, copyright 2026 Wookyung Chung), matching the upstream
  open-knowledge-format project.
- Normalized all concept cross-links to bundle-root-relative
  (`/domain/concept.md`, the OKF viewer convention) and migrated
  `reviewed_by`/`reviewed_at` to the spec `verified` key, so concepts
  derive the `human-reviewed` trust tier (§5.3).
- Added the static viewer site: `site/build.py` plus the vendored OKF
  visualizer (`site/vendor/`, upstream `ad30107`, patched link
  extraction and palette) render 41 concepts / 142 edges to a
  self-contained `index.html`; `.github/workflows/site.yml` deploys it
  to `gh-pages`, served at
  `https://api.friedman.jp/MacroEconometricModels-OKF/`.
- Verification pass: all 39 concepts re-checked against upstream
  `3d12bb6c` (multi-agent sweep plus inline re-verification of
  truncated scopes). No invented names found. Fixed 14 MEDs (wrong
  estimator claims in `nonlinear`, `occbin_irf` signature, `did`
  method symbols, `nowcast` horizons, `system`/`reg` over-broad
  uniformity claims, `dgp` hetero kinds, `:wiot` identity, `vecm`
  `predict` levels, `factor` StatsAPI gaps, `dsge` file count and
  `rank_rtol` forwarding, `ct` default, `lp` variant count) and ~25
  LOWs (omitted rows/fields, stale counts, weak see-also links, two
  dead plain-text refs in `nonlinear`/`statespace`). A few
  agent-reported LOW details did not reproduce against upstream and
  were left unchanged.
- Dogfood fixes: all bundle links converted from bundle-rooted to
  relative paths so navigation works both in the viewer and on GitHub
  (the validator now checks relative links too, with a negative
  control); hub links retargeted to lead concepts; four plain-text
  See also blocks converted to markdown links; three topical in-links
  added (`pvar`→`gmm`, `reg`→`nonparametric`, `lp`→`counterfactual`).
  Viewer upgrades: body/description search, domain filter, per-concept
  view-source links, `status-approved` badge style, relative-link
  resolution, responsive layout, CDN-load guard, `log.md` excluded
  from the graph. Result: 40 concepts, 171 edges, zero orphans,
  zero dead in-viewer links.
