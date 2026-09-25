---
type: Feature
title: Input-Output Analysis (IO)
description: Leontief and Ghosh input-output analysis with multipliers, linkages, environmental extensions, MRIO trade accounting, Baqaee-Farhi production networks, and MRIO data downloaders.
resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/io
tags:
  - io
  - input-output
  - mrio
  - multipliers
  - linkages
  - production-networks
  - trade
status: approved
verified: { by: human:chung9207, at: 2026-09-25T02:10:00Z }
stale_after: 2026-12-24T00:00:00Z
generated:
  by: memecon-okf/small-domains
  at: 2026-09-25T01:25:44Z
sources:
  - id: io-hub
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io.md
    title: Input-Output Analysis docs hub
  - id: io-classical
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io_classical.md
    title: Classical Input-Output Analysis docs
  - id: io-environmental
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io_environmental.md
    title: Environmental Extensions docs
  - id: io-mrio
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io_mrio.md
    title: MRIO Trade Accounting docs
  - id: io-baqaee-farhi
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io_baqaee_farhi.md
    title: Baqaee and Farhi Nonlinear Input-Output docs
  - id: io-download
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/docs/src/io_download.md
    title: Downloading IO/MRIO Data docs
  - id: src-io
    resource: https://github.com/FriedmanJP/MacroEconometricModels.jl/tree/3d12bb6c/src/io
    title: src/io module source
---

# Summary

The `io` module implements demand-driven (Leontief 1936) and supply-driven (Ghosh 1958) input-output analysis over a single `IOData` container holding intermediate flows `Z`, final demand `Y`, value added `va`, gross output `x`, labels, and satellite accounts, with row and column accounting balances validated at construction.
Classical tools (multipliers, Rasmussen linkages, SDA, RAS/GRAS, hypothetical extraction, price dual, impact scenarios, network statistics) are linear in the Leontief inverse; environmental extensions push satellite accounts through that same inverse for consumption-based footprints; the MRIO layer adds KWW (2014) export decompositions; and the Baqaee-Farhi layer reinterprets the table as a nested-CES production network for exact nonlinear counterfactuals.
Downloaders fetch the public MRIO databases (OECD ICIO, WIOD, EXIOBASE 3, GLORIA) with SHA-256 verification against the `IO_CHECKSUMS` registry — which ships unpopulated, so downloads warn as unverified until the user registers digests — while EORA26 has no automated fetch (manual download; the function throws), and `parse_icio` / `parse_wiod` turn archives into `IOData`.

# Functions

| Function | Signature | Role |
|---|---|---|
| [IOData](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/types.jl) | `IOData(Z, Y, va; sectors, regions, fd_cats, va_cats, unit, year, source, meta, check)` | Container constructor; derives `x` from the row balance and validates both balances |
| [technical_coefficients](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `technical_coefficients(io::IOData)` | Technical-coefficients matrix `A = Z x̂⁻¹` |
| [leontief_inverse](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `leontief_inverse(io::IOData)` | Total-requirements matrix `L = (I - A)⁻¹` |
| [allocation_coefficients](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `allocation_coefficients(io::IOData)` | Allocation-coefficients matrix `B = x̂⁻¹ Z` |
| [ghosh_inverse](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `ghosh_inverse(io::IOData)` | Supply-driven inverse `G = (I - B)⁻¹` |
| [leontief](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `leontief(io::IOData)` | Bundled coefficients, inverse, output, and table back-reference |
| [ghosh](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `ghosh(io::IOData)` | Bundled allocation coefficients, Ghosh inverse, and output |
| [multipliers](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/multipliers.jl) | `multipliers(io::IOData; kind=:output, type=:I)` | Output, income, or employment multipliers, Type I (open) or Type II (closed) |
| [linkages](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/linkages.jl) | `linkages(io::IOData; forward=:ghosh)` | Backward/forward linkages, Rasmussen indices, and key-sector classification |
| [key_sectors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/linkages.jl) | `key_sectors(io::IOData)` | Per-sector `:key` / `:forward` / `:backward` / `:weak` classification (`rasmussen` is an alias of `linkages`) |
| [sda](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/sda.jl) | `sda(io0::IOData, io1::IOData; method=:additive, factors=nothing, on=:output)` | Additive n-factor or multiplicative two-factor structural decomposition |
| [ras](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/ras.jl) | `ras(A0, u, v; tol=1e-10, maxiter=1000)` | Biproportional RAS rebalance of a matrix to new margins |
| [gras](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/ras.jl) | `gras(A0, u, v; tol=1e-10, maxiter=1000)` | Generalized RAS allowing negative entries |
| [balance](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/ras.jl) | `balance(io::IOData; method=:ras, tol=1e-10, maxiter=1000)` | Repair a table's intermediate flows via RAS/GRAS |
| [hypothetical_extraction](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/extraction.jl) | `hypothetical_extraction(io::IOData, sectors; mode=:complete, share=1.0, region=nothing)` | Complete, backward, forward, or partial extraction loss of a sector |
| [price_model](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/price.jl) | `price_model(io::IOData; dva=nothing, dtax=nothing, mode=:leontief)` | Leontief cost-push price dual `Δp = (I - A')⁻¹ Δv` |
| [impact](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/impact.jl) | `impact(io::IOData, dy; kind=:output, type=:I, fix=Dict())` | Final-demand shock scenario through `L`, with Type II and mixed-model options |
| [network_stats](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/network.jl) | `network_stats(io::IOData)` | Domar concentration, average propagation lengths, degree structure |
| [add_extension!](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/environmental.jl) | `add_extension!(io::IOData, name, F::AbstractMatrix; stressors, unit, F_Y=nothing)` | Attach a satellite account (physical flows) to the table in place |
| [intensities](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/environmental.jl) | `intensities(io::IOData, name::AbstractString)` | Direct stressor intensities `S = F x̂⁻¹` of an extension |
| [emission_multipliers](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/environmental.jl) | `emission_multipliers(io::IOData, name::AbstractString)` | Consumption-based multipliers `M = S L` of an extension |
| [footprint](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/environmental.jl) | `footprint(io::IOData, name::AbstractString; by=:sector)` | Consumption-based footprint by sector (`:sector`) or production-vs-consumption by region (`:region`) |
| [aggregate](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `aggregate(io::IOData; region_map=nothing, sector_map=nothing)` | Collapse regions and sector types, carrying satellite accounts along |
| [region_block](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `region_block(io::IOData, r, s)` | `N × N` intermediate block from supplying region `r` to using region `s` |
| [region_indices](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `region_indices(io::IOData, region)` | Row/column indices of a region (name or 1-based index) |
| [bilateral_trade](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `bilateral_trade(io::IOData, exporter, importer; kind=:total)` | Bilateral intermediate, final, and total exports |
| [gross_exports](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `gross_exports(io::IOData, region)` | Sectoral exports of a region to all other regions |
| [vertical_specialization](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `vertical_specialization(io::IOData, region=nothing)` | Hummels-Ishii-Yi import content of exports |
| [export_decomposition](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/mrio.jl) | `export_decomposition(io::IOData, region=nothing)` | Koopman-Wang-Wei (2014) DVA/RDV/FVA/PDC export decomposition |
| [domar_weights](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/baqaee_farhi.jl) | `domar_weights(io::IOData)` | Domar weights (sales over GDP); first-order GDP elasticities via Hulten's theorem |
| [baqaee_farhi](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/baqaee_farhi.jl) | `baqaee_farhi(io::IOData; theta=nothing, sigma=nothing)` | Second-order "beyond Hulten" Hessian on a table under scalar elasticities |
| [production_network](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_network.jl) | `production_network(io::IOData; theta=1.0, sigma=1.0, epsilon=1.0, eta=1.0, ...)` | Standard-form nested-CES `ProductionNetwork` with heterogeneous elasticities and markups |
| [baqaee_farhi](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_hessian.jl) | `baqaee_farhi(net::ProductionNetwork; hessian=:auto, elasticities=true)` | Generalized multi-factor local approximation on a standard-form network |
| [bf_equilibrium](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_equilibrium.jl) | `bf_equilibrium(net::ProductionNetwork; dlogA=nothing, dlogL=nothing, dlogmu=nothing, method=:newton, ...)` | Exact nonlinear equilibrium for large productivity, factor-supply, and markup shocks |
| [bf_elasticities](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_hessian.jl) | `bf_elasticities(net::ProductionNetwork)` | Factor-price, goods-price, and Domar-share incidence of productivity shocks |
| [bf_quadratic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_hessian.jl) | `bf_quadratic(net::ProductionNetwork, v::AbstractVector)` | Second-order GDP change along a productivity direction |
| [bf_shock_curve](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_hessian.jl) | `bf_shock_curve(net::ProductionNetwork, sector; range=(-0.5, 0.5), points=41, ...)` | Exact vs Hulten vs second-order GDP response over a shock range |
| [bf_wedge_decomp](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_wedges.jl) | `bf_wedge_decomp(net::ProductionNetwork; dlogA=nothing, dlogmu=nothing, dlogL=nothing, ...)` | B&F (2020) Theorem 1 technology vs allocative-efficiency decomposition |
| [cost_based_domar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_wedges.jl) | `cost_based_domar(net::ProductionNetwork)` | Cost-based Domar weights under markups |
| [revenue_based_domar](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_wedges.jl) | `revenue_based_domar(net::ProductionNetwork)` | Revenue-based Domar weights under markups |
| [bf_misallocation](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_misalloc.jl) | `bf_misallocation(net::ProductionNetwork; point=:efficient, hessian=:auto)` | Proposition 5 Harberger misallocation from markups |
| [bf_wedge_quadratic](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/bf_misalloc.jl) | `bf_wedge_quadratic(net::ProductionNetwork, v::AbstractVector; point=:efficient)` | Second-order GDP change along a markup direction |
| [list_io_sources](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/download/registry.jl) | `list_io_sources()` | Catalog of the five fetchable MRIO sources with versions and credential needs |
| [download_io](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/download/sources.jl) | `download_io(source::Symbol; storage_folder, years=nothing, ...)` | Dispatching downloader returning an `IOMetaData` provenance log |
| [download_oecd](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/download/sources.jl) | `download_oecd(folder; version="v2023", years=nothing, ...)` | Per-source downloaders (`download_oecd`, `download_wiod`, `download_exiobase3`, `download_eora26`, `download_gloria`); EORA26 throws with the manual route |
| [parse_io](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/parse.jl) | `parse_io(path::AbstractString; source::Symbol, year=nothing, ...)` | Parse a delimited table (CSV/TSV) or dispatch ZIP/XLSX to extensions |
| [parse_icio](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/parse.jl) | `parse_icio(path::AbstractString; year=nothing, member="", ...)` | Labeled MRIO recipe for OECD ICIO archives |
| [parse_wiod](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/parse.jl) | `parse_wiod(path::AbstractString; year=nothing, sheet=1, ...)` | Labeled MRIO recipe for WIOD 2013 workbooks |
| [io_file_digest](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/download/registry.jl) | `io_file_digest(path::AbstractString)` | SHA-256 hex digest of a downloaded archive for the integrity registry |
| [nsectors](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/types.jl) | `nsectors(io::IOData)` / `nregions(io::IOData)` | Sectors-per-region and region count of a table |
| [LeontiefModel](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/io/coefficients.jl) | `struct LeontiefModel{T}` / `struct GhoshModel{T}` | Bundled inverses returned by `leontief` / `ghosh` |
| [plot_result](https://github.com/FriedmanJP/MacroEconometricModels.jl/blob/3d12bb6c/src/plotting/io.jl) | `plot_result(r::Union{ExtractionResult,PriceModelResult,ImpactResult,...}; title, save_path)` | Plots for extraction, price, impact, network, multiplier, linkage, and equilibrium results |

Result types (`IOMultipliers`, `LinkageResult`, `SDAResult`, `RASResult`, `ExtractionResult`, `PriceModelResult`, `ImpactResult`, `NetworkStatsResult`, `FootprintResult`, `RegionalFootprintResult`, `VerticalSpecialization`, `ExportDecomposition`, `BaqaeeFarhiResult`, `BFLocal`, `BFElasticities`, `BFShockCurve`, `BFWedgeDecomp`, `BFMisallocation`, `BFEquilibrium`, `IOExtension`, `IOMetaData`) are omitted from the table; see `src/io` for their fields.

# Examples

```julia
using MacroEconometricModels
io = load_example(:wiot)
multipliers(io; kind=:output, type=:I)
linkages(io)
footprint(io, "CO2")
```

# See also

* [Data Management](/infrastructure/data.md) - home of the `:wiot` example table and container conventions
