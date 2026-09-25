## Goal

Publish an OKF v0.2 knowledge bundle, in this repository, that describes each
feature area and function of
[MacroEconometricModels.jl](https://github.com/FriedmanJP/MacroEconometricModels.jl),
so that humans can browse it and agents can load it into context.

## Success Criteria

- The repository is a conformant OKF v0.2 bundle: every concept file carries
  parseable YAML frontmatter with a non-empty `type`; reserved files follow
  the index/log structure.
- Every upstream feature area (all 39 `src/` module directories) is covered
  by exactly one `type: Feature` concept; the bundle root carries a
  `type: Package` overview and an `index.md` declaring `okf_version: "0.2"`.
- Every concept records provenance (`sources` pinned to an upstream commit),
  authorship (`generated`), and lifecycle (`status`); no concept stays
  `draft` without a recorded reason.
- A CI check validates conformance on every push; `main` is green.
- A human reviewer has marked core concepts `verified`, promoting them to the
  human-reviewed trust tier.

## Context And Current Facts

- Upstream package (sibling checkout `../MacroEconometricModels`, HEAD
  `3d12bb6c`, version 1.0.0): 39 `src/` module directories (~458 `.jl`
  files), 83 `docs/src/*.md` pages, 442 `export` lines, GPL-3.0 licensed.
  Largest modules: `dsge` (63 files), `teststat` (53), `plotting` (42),
  `core` (30), `io` (23), `reg` (22).
- OKF v0.2 (spec inspected this run): a bundle is a directory tree of
  Markdown files with YAML frontmatter; `type` is the only required key;
  `title`/`description`/`resource`/`tags` are recommended; `sources`,
  `generated`/`verified`, `status`/`stale_after` are the optional
  provenance/trust/lifecycle families; `Attested Computation` is an
  optional per-computation concept type; `index.md`/`log.md` are reserved
  filenames for progressive disclosure and history; cross-links SHOULD be
  bundle-relative (`/dir/concept.md`); consumers MUST tolerate unknown
  types, extra keys, and broken links.
- This repository (`MacroEconometricModels-OKF`, empty at session start)
  is intended as the bundle itself, published under the `FriedmanJP` GitHub
  organization. The requester holds an admin role there, and the repo name
  is free.
- Docs pages outnumber module directories (83 vs 39) because guides
  (getting-started, method guides, identification testing) cut across
  modules; the bundle therefore needs both feature concepts and guide
  concepts.

## Constraints And Non-goals

- The bundle describes upstream; it never patches it. Upstream drift is
  handled by refreshing concepts, not by forking code.
- OKF conformance is the ceiling for format strictness: no custom required
  fields beyond `type`, no bespoke tooling a reader must install.
- Non-goals for this plan: per-function concept files for all ~1000+
  exported identifiers (Phase 2 covers feature-level tables; per-function
  explosion is an explicitly optional Phase 2b); a served UI or search
  index over the bundle; Attested Computations as a deliverable (optional
  Phase 3 pilot only).

## Key Decisions

- D1 — Bundle at repo root. The repository IS the bundle (the spec allows
  a bundle to be a git repository), with `okf_version: "0.2"` in the root
  `index.md`. Rejected: a `bundles/memecon/` subdirectory, which adds
  nesting for a second bundle that is not planned.
- D2 — One concept per feature area in Phase 2 (~39 files), each with a
  function table in the body, not one file per function. Rationale: 442
  export lines imply 1000+ identifiers; file-per-function is unreviewable
  as a first pass and index files already give progressive disclosure.
- D3 — Minimal producer-defined type taxonomy: `Package` (one overview),
  `Feature` (one per feature area), `Guide` (workflow how-tos mirroring
  cross-cutting docs), `Reference` (notation, bibliography pointers, API
  tables). The spec fixes no taxonomy and consumers must tolerate unknown
  types, so starting small is safe.
- D4 — Twelve domain directories mirroring the package's own feature
  groups: `univariate/`, `nonlinear-statespace/`, `multivariate/`,
  `panel/`, `dsge/`, `io/`, `policy-counterfactuals/`, `cross-section/`,
  `nonparametric/`, `forecasting/`, `testing/`, `infrastructure/`, plus
  `guides/` and `references/`. Each domain directory gets an `index.md`.
- D5 — Provenance pinned to upstream commit SHAs (no `v1.0.0` tag exists
  yet, so tags cannot be cited); `generated.by` names the authoring agent;
  `status: draft` until human review flips it to `stable`; `stale_after`
  set to 90 days after generation and refreshed by the maintenance pass.
- D6 — Authoring pipeline is agent-draft plus human-review-gate: drafts
  are written from upstream `docs/src` pages (primary) and `src`
  docstrings (secondary); nothing reaches `stable`/`verified` without a
  human pass. `log.md` is updated once per phase, not per file.
- D7 — License mirrors upstream (GPL-3.0). Reversible: a docs-only bundle
  could move to a documentation license later without touching content.

## Recommended Approach

Scaffold the bundle skeleton with indexes first, then author in three
passes of increasing detail (pilot → full feature coverage → cross-links
and optional extras), validating OKF conformance in CI from the first
concept onward. Front-load the two highest-risk modules (`dsge`,
`teststat`) into the pilot so granularity and table conventions are proven
on the hardest material before the bulk pass. Keep every phase's output
reviewable as a single pull request.

## Work Plan

### Phase 0 — Repo scaffold (done, pre-approval)

- Initialize this repo, publish it to `FriedmanJP/MacroEconometricModels-OKF`,
  commit `README.md`, `LICENSE`, and this plan. No OKF concepts yet.

### Phase 1 — Skeleton + pilot (1 PR)

- Add root `index.md` (with `okf_version`), root `log.md`, `overview.md`
  (`type: Package`), and domain `index.md` stubs for all 12 domains.
- Author 4 pilot `type: Feature` concepts spanning difficulty: `dsge`
  (largest, solver-heavy), `teststat` (largest flat surface), `var`
  (small, canonical estimator), `arima` (small, univariate).
- Add the conformance-check CI job (parses every frontmatter block,
  asserts non-empty `type`, checks reserved-file structure and that
  bundle-relative links resolve).
- Owner/surface: bundle root + `dsge/`, `testing/`, `multivariate/`,
  `univariate/` + `.github/workflows/`.

### Phase 2a — Full feature coverage (1–2 PRs)

- Author the remaining ~35 `type: Feature` concepts, one per `src/`
  directory, following the pilot's body convention (`# Summary`,
  `# Functions` table with signature + one-line role + upstream link,
  `# Examples` with a minimal Julia snippet adapted from docs, `# See also`
  with bundle-relative links).
- Fill in all domain `index.md` descriptions from concept frontmatter.
- Update root `log.md`.

### Phase 2b — Per-function explosion (OPTIONAL, only if approved)

- Expand flagship APIs (e.g. VAR/VECM/LP/DSGE estimation entry points)
  into `type: Reference` per-function concepts. Otherwise the feature
  tables remain the function-level record.

### Phase 3 — Guides, references, attestation pilot (1 PR, partially optional)

- Author `Guide` concepts for cross-cutting docs (getting-started,
  identification strategy, model selection, forecasting workflow).
- Add `references/` pointers (notation, bibliography) per the OKF
  convention.
- OPTIONAL: one `Attested Computation` pilot (e.g. a canonical estimator
  call with `runtime: julia`), which requires designing the executor /
  attester harness the spec leaves to the producer.

### Phase 4 — Maintenance wiring (1 PR)

- Scheduled job that diffs pinned upstream SHAs against upstream `main`
  and opens an update issue when they drift; `stale_after` refresh policy
  (`+90d` on each confirmed-fresh pass) documented in `log.md`.

## Validation Plan

- After every phase: the CI conformance job passes on the PR (frontmatter
  parses, `type` present everywhere, reserved files well-formed, all
  bundle-relative links resolve to existing files). Highest-risk step: the
  link check on Phase 2a, when ~40 files land at once — run the checker
  locally before pushing.
- Content check per concept (manual, reviewer): the `# Functions` table
  names resolve against the pinned upstream SHA (spot-check 3 identifiers
  per concept via the upstream GitHub blob URL in `sources`); the
  `# Examples` snippet is copied from, or smoke-tested against, upstream
  docs.
- Trust check before any `stable` flip: `verified: { by: human:<id>, ... }`
  present, `status: stable`, `stale_after` in the future.
- Final acceptance: clone the repo fresh, confirm `index.md` at root and
  in each domain renders a complete progressive-disclosure path from root
  to every concept with no stub left behind (except explicitly deferred
  Phase 2b/attestation items).

## Risks / Rollback

- Upstream drift (package is under active development): mitigated by
  SHA-pinned `sources` plus the Phase 4 drift job. Rollback: revert the
  stale concept to `status: draft` with a `log.md` entry; never delete
  (links must keep resolving).
- Granularity mismatch (tables too coarse for real agent use): mitigated
  by piloting on the two hardest modules first; escape hatch is Phase 2b.
- Review bottleneck (39 concepts × human gate): mitigated by batching
  Phase 2a into two PRs by domain and allowing `draft`+unverified merge
  with `stable` flips following asynchronously.
- No code ships, so rollback is `git revert` on docs-only commits; the
  upstream package is untouched by every phase.

## Open Questions

- Q1: Who is the human verifier recorded in `verified.by`? Default
  assumption: `human:chung9207`.
- Q2: Keep GPL-3.0, or prefer a documentation license for the bundle?
  Default assumption: keep GPL-3.0 (mirrors upstream, reversible).
- Q3: Is feature-level coverage (Phase 2a) sufficient, or is the
  per-function explosion (Phase 2b) wanted? Default assumption: decide
  after reviewing the pilot.

## Sources

- <https://raw.githubusercontent.com/GoogleCloudPlatform/open-knowledge-format/main/SPEC.md>
- <https://raw.githubusercontent.com/GoogleCloudPlatform/open-knowledge-format/main/README.md>
