# cbefs-model-bgc-repo

The CBEFS **biogeochemistry** — a data repository of the oceansensing ocean map system: its own
Pages site, its own schedule, its own gigabyte, holding no code of its own.

**Live since 2026-09-27** — published to Pages and R2, not drawn on the
website's map. `PLAN.md` is the founding plan;
`CLAUDE.md` carries what must not be got wrong and the shared doc doctrine.

## What it publishes

The Chesapeake Bay Environmental Forecasting System's **biogeochemistry**:
dissolved oxygen, pH, total alkalinity and aragonite saturation state
(Ω_arag), each at the surface and the bottom.

| root | quantity | unit |
| --- | --- | --- |
| `o2-cbefs.json`, `o2-cbefs-bottom.json` | dissolved oxygen | mmol m-3 |
| `ph-cbefs.json`, `ph-cbefs-bottom.json` | pH | 1 |
| `talk-cbefs.json`, `talk-cbefs-bottom.json` | total alkalinity | mmol m-3 (the file's meq m-3) |
| `omega-cbefs.json`, `omega-cbefs-bottom.json` | aragonite saturation state | 1 |

The daily mean stamped noon UTC on today's date. About 0.8 MB each, 6.1 MB a
tree (measured 2026-09-27).

## Published to R2 alone (since 2026-10-10)

Declared `r2_only` in `pipeline/products.toml`: the same run builds these,
they are left out of this repository's Pages site and its status, and the R2
job publishes them beside the rest (the site pipeline's D13, its note of
2026-10-09). Their roots stay on the `published` branch, as every product's do.

| root | quantity | unit |
| --- | --- | --- |
| `no3-cbefs-<depth>m.json` | nitrate at each of the first 17 of Mercator's depths, 0.494 to 40.344 m, interpolated from the model's 20 terrain-following levels below the datum, daily means; one root a depth named for it to the meter (`-0m` … `-40m`) | mmol m-3 |
| `nh4-cbefs-<depth>m.json` | ammonium at the same depths | mmol m-3 |
| `po4-cbefs-<depth>m.json` | phosphate at the same depths | mmol m-3 |
| `oxygen-cbefs-<depth>m.json` | dissolved oxygen at the same depths | mmol m-3 |

The nutrients dip slightly below zero where the model undershoots (nitrate to
−1.3 mmol m-3, measured on the 2026-10-10 run): the model's values, published
as they are.

Every root is one regional grid at 0.007 degree (336 x 438, `regional:
true`), `source: Chesapeake Bay Environmental Forecast System (CBEFS),
Virginia Institute of Marine Science` — the citation the data's license asks
for. **A bottom root is s-level 0** (ROMS counts from the seabed up), and its
header carries no depth, as Mercator's `bottomt` does not. The fetcher is the
site's `scripts/fetch-cbefs.py`, shared by the three CBEFS repositories and
scoped here with `--only=`; the workflow runs on `27 1,7,13,19 * * *` since
2026-09-27, after its first dispatched run published.

These products are published **operationally but not drawn on the website's
map** — the owner's call, 2026-09-27. The map's status line still reports
them when they fall behind, which is how their health stays visible.

## Where the data comes from

**Source, read 2026-09-26/27**: VIMS publishes CBEFS output openly, with no
login, through THREDDS/OPeNDAP at
`https://tds.vims.edu/thredds/catalog/abever/ECB_FORECAST_HR/catalog.xml`,
in daily folders (`AVERAGES/`, `HISTORY/`, `SURFACE_VELOCITY/`, `STATION/`).
The model is ROMS ChesROMS-ECB on a **curvilinear** 336 × 564 grid at about
600 m with 20 terrain-following levels; it runs nightly for one nowcast day
and five forecast days, and each day's files were posted around 12:20 UTC.
The daily-average file carries `temp`, `salt`, `oxygen`, `pH`, `alkalinity`
and `Aragonite` on every level, so the surface and the bottom are the top and
bottom s-levels, fetched by OPeNDAP index subsetting; `SURFACE_VELOCITY`
carries hourly surface currents already on rho points and eastward/northward.
The files' own license: *"These data are freely available for public use.
Please cite the Chesapeake Bay Environmental Forecast System (CBEFS), Virginia
Institute of Marine Science, when using this data."* The citation is carried
in every published header's `source` and in the README.

## How it runs

The orchestrator (the site's private `pipeline/`), the fetchers and the
published-file contract all come from `oceansensing.github.io`, checked out at
run time. This repository carries `pipeline/products.toml` and its publish
workflow, and nothing else executable. Each run publishes to GitHub Pages and
to Cloudflare R2 from one build. Sibling repositories of the same model:
`cbefs-model-fields-repo` and `cbefs-model-currents-repo`.

**Which document gets what, and what "update docs" means across all
twenty repositories, is the doctrine block at the top of `CLAUDE.md`** —
the same text in all twenty, held equal by the site's `check:docs`.

## Structure

```
README.md       what this is
CLAUDE.md       what must not be got wrong, and the shared doc doctrine
PLAN.md         the founding plan and running record
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml, the declaration the orchestrator reads
.github/        the publish workflow
```
