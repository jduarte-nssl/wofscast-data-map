# WoFSCast Data Pipeline Map

An interactive visualization and reference guide tracing the full data chain behind
**WoFSCast** — from raw physical observations all the way to probabilistic storm-scale
AI forecasts — including WoFS-WRF, WoFS-MPAS, and all upstream models and observations.

---

## Project Files

| File | Description |
|------|-------------|
| `index.html` | Interactive web application — self-contained, no server required |
| `wofscast_data_pipeline.md` | Full reference: Mermaid diagrams, tables, and narrative guide |
| `README.md` | This file |

---

## Interactive Web App — `index.html`

Open `index.html` in any modern browser. No installation needed.
On first open, [vis.js](https://visjs.org/) is fetched from CDN; after that it works fully offline.

### Overview

A hierarchical, directed flow graph of **45 nodes** and **76+ edges** tracing the complete
WoFSCast data chain across 8 tiers, from physical sensors at the top to forecast products
at the bottom.

### Node Categories

| Category | Node Shape | Color | What's in it |
|----------|-----------|-------|--------------|
| Physical Sensors | Box | 🔵 Blue | WSR-88D/NEXRAD, GOES-16/18/19 (ABI+GLM), Polar-orbiting satellites (ATMS/CrIS/IASI), GNSS-RO (COSMIC-2/Spire), Radiosondes, ASOS/METAR, Oklahoma & National Mesonet, Aircraft ACARS/AMDAR |
| Raw Processing | Box | ⬜ Slate | RPG (Level-III products), GOES L1b calibration, GOES L2 products (CWP/CTH/cloud mask), PrepBUFR/IODA formatting, MRMS national mosaics |
| Static & Ancillary Data | Box | 🟢 Green | Terrain/DEM (GTOPO30/SRTM), Land Use/LULC (MODIS/USGS), Sea Surface Temperature (RTG-SST) |
| Global Models | Box | 🟡 Yellow | GDAS (GSI 4DEnVar), GFS/FV3-GFS, GEFS, ERA5 Reanalysis (ECMWF), ECMWF IFS & AIFS |
| Regional Models | Box | 🟠 Orange | RAP (13 km, GSI 3DEnVar), HRRR (3 km, GSI+radar DA), HRRRe (6-member ensemble) |
| WoFS NWP Systems | Box | 🟩 Teal | WRF Pre-processing (WPS), MPAS Pre-processing, WoFS-WRF DA (GSI-EnKF), WoFS-WRF Ensemble (36 members), WoFS-MPAS DA (DART/EAKF — primary), WoFS-MPAS DA (JEDI — experimental), WoFS-MPAS Ensemble (36 members) |
| ML / WoFSCast | Diamond | 🩷 Pink | Training pipeline, GNN model (GraphCast-style), Real-time inference, Forecast products |
| Future / Planned | Hexagon | 🟨 Amber | RRFS/REFS (Oct 2026), NOAA EAGLE/AIGFS (Dec 2025), GeoXO (~2030s), Phased Array Radar, WoFSCast v2 |

**Solid arrows** = current operational data flow.
**Dashed arrows** = future, experimental, or potential connections.

---

### App Features

#### Click any node → Info sidebar

Each node opens a sidebar panel with:

- **Category badge** — color-coded tier label
- **Description** — what the system is, how it works, its role in the chain
- **Inputs** — what feeds into this node
- **Outputs / Products** — what it produces and where those products go
- **⚡ Future Changes** — planned retirements, transitions, or upgrades
- **Primary source links** — clickable links to official docs, open data portals, and key papers

The node sidebar also shows a **connection panel** with four sections:
- **⬆ Incoming connections** — direct edges flowing into this node (click to open edge sidebar)
- **⬇ Outgoing connections** — direct edges flowing out of this node (click to open edge sidebar)
- **⏫ Upstream nodes** — direct source nodes (click to focus that node)
- **⏬ Downstream nodes** — direct target nodes (click to focus that node)

#### Click any edge → Edge sidebar

Clicking any arrow on the graph opens an edge sidebar showing:

- **Color-coded type badge** — edge category (e.g., realtime, historical, future)
- **Description** — what the data connection represents and its role in the pipeline
- **Flow diagram** — vertical from-node → connector → to-node visualization; both nodes are clickable
- **Data type tags** — the kinds of data carried by this connection
- **Sources & references** — links to relevant documentation

#### Isolate mode

Each node sidebar includes an **Isolate** toggle slider. When enabled, the graph filters
to show only the selected node and all of its transitive upstream ancestors (traced back
through the full edge graph via BFS). Toggling off restores the full view. Isolate mode
also resets automatically when closing the sidebar, changing the category filter, or
switching view mode.

#### Category filter buttons

Narrow the graph to one or more tiers using the buttons in the top bar.
**Multiple categories can be selected simultaneously** — click any combination of category
buttons to show only those tiers together. Clicking "All" resets to the full graph.
If all individual categories are deselected, the view automatically reverts to "All".
Useful for focusing on just the sensor layer, comparing WoFS systems with global models,
or isolating all future/planned nodes.

#### Live search

Type in the search box to highlight matching nodes and dim the rest.
Searches across node name, subtitle, and full description text.

#### Reset view

Fits the full graph back into the window after zooming or panning.

#### Navigation
- **Scroll / pinch** — zoom in and out
- **Click + drag** — pan the canvas
- **Click a node** — open info sidebar
- **Click an edge** — open edge sidebar
- **Click background** — close sidebar / deselect

---

## Reference Document — `wofscast_data_pipeline.md`

A 1,100+ line Markdown reference document (67 KB) organized into four major sections.

### Section 1 — Current Pipeline Mermaid Diagram

A full `flowchart TD` Mermaid diagram tracing:
- All physical sensors → raw processing → MRMS
- GDAS/GFS/GEFS global model chain
- RAP → HRRR → HRRRe regional chain
- Static data → WPS / MPAS pre-processing
- WoFS-WRF (GSI-EnKF) and WoFS-MPAS (DART/EAKF + JEDI) DA systems
- Both WoFS NWP ensembles → WoFSCast training and inference pipelines

### Section 2 — Future Pipeline Mermaid Diagram

A separate diagram showing anticipated changes:
- GOES-19 transition (✅ complete, April 2025)
- NOAA EAGLE / AIGFS / AIGEFS (✅ operational December 2025)
- ECMWF AIFS Single + ENS (✅ operational 2025)
- **RRFS + REFS replacing HRRR/RAP/HRRRe (October 6, 2026)**
- WoFS-RRFS (planned), WoFSCast v2 diffusion ensemble
- GeoXO constellation (~early 2030s)
- Phased Array Radar (PAR, TBD)
- Other global AI models (Pangu-Weather, FourCastNet, GraphCast)

### Section 3 — Reference Tables

| Table | Contents |
|-------|----------|
| Physical Sensors | Platform, key variables, data format, update rate |
| Global Models | Core, DA method, resolution, output format |
| Mesoscale Models | Core, DA method, resolution, boundary condition source |
| WoFS-WRF vs. WoFS-MPAS | DA framework, BCs, grid type, output format |
| WoFSCast ML System | Training, architecture, inference, output |
| DART/EAKF vs. JEDI | Maturity, obs types, all-sky support, code stack |
| Future Changes | Near-term (2025–2026), medium-term (2027–2030), long-term (2030+) |
| ECMWF/International AI Models | Role of ERA5, AIFS, IFS, GraphCast, Pangu in WoFSCast ecosystem |

### Section 4 — Narrative Guide (Parts 1–10)

Detailed written explanation of every pipeline block:

| Part | Coverage |
|------|----------|
| 1 — Physical Sensors | WSR-88D Level-II/III pipeline, dual-pol variables, MRMS as DA gateway; GOES ABI L0→L1b→L2 chain; polar-orbit sounders (AMSU-A, ATMS, CrIS, IASI); GNSS-RO; radiosonde network; ASOS/Mesonet; aircraft ACARS; PrepBUFR vs. IODA formats |
| 2 — Global Models | GDAS 6-hourly Hybrid 4DEnVar cycle; how GFS errors propagate through the chain; GEFS ensemble perturbations for WoFS-MPAS BCs |
| 3 — Mesoscale Models | RAP GSI 3DEnVar hourly cycle; HRRR 15-min radar DA sub-cycle and reflectivity nudging; HRRRe as WoFS-WRF IC/BC source |
| 4 — Static Data | Why terrain, land use, soil, and SST matter for convective initiation in WoFS domains |
| 5 — Pre-processing | WPS geogrid/ungrib/metgrid workflow; MPAS init_atmosphere and Voronoi mesh advantages over WRF nested grids |
| 6 — WoFS DA Systems | GSI-EnKF flow-dependent covariances; DART/EAKF deterministic ensemble update; JEDI all-sky radiance DA via CRTM; IODA observation format |
| 7 — WoFS NWP Ensembles | WoFS-WRF physics perturbation strategy; WoFS-MPAS Voronoi mesh variable-resolution advantages |
| 8 — WoFSCast ML | Variable selection, icosahedral regridding, normalization, time-pair construction, graph mesh building; GNN encoder–processor–decoder architecture; 30-second inference vs. 10-minute NWP |
| 9 — Alternative Pipelines | WoFS-WRF vs. MPAS comparison; DART vs. JEDI detailed feature table; GSI vs. JEDI transition roadmap |
| 10 — Future Changes | RRFS/REFS Oct 2026 and all downstream WoFS consequences; NOAA EAGLE AIGFS/AIGEFS; ERA5 and AIFS role; GeoXO hyperspectral sounder; PAR 30-second scans; WoFSCast diffusion ensemble and 1-km roadmap |

---

## Full Pipeline Summary

```
┌─────────────────────────────────────────────────────────────────┐
│  PHYSICAL SENSORS                                               │
│  WSR-88D (160 sites) → RPG → MRMS (1-km, 2-min mosaics)        │
│  GOES-19/18 ABI → L1b → L2 (CWP, CTH, cloud mask)             │
│  Polar sats (ATMS/CrIS/IASI) → BUFR radiances                  │
│  GNSS-RO (COSMIC-2, Spire) → refractivity profiles             │
│  Radiosondes / ASOS / Mesonet / Aircraft → PrepBUFR            │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│  GLOBAL MODELS                                                  │
│  GDAS (GSI Hybrid 4DEnVar, 6-hourly) → GFS/FV3 (~13 km)        │
│  GFS → GEFS (30-member ensemble, ~25 km)                        │
│  ERA5 (ECMWF reanalysis, 1940–present) — ML training data       │
│  ECMWF IFS/AIFS — potential future BC source                    │
└──────────────┬──────────────────────────┬───────────────────────┘
               │ LBCs                     │ Ensemble BCs
┌──────────────▼──────────┐   ┌───────────▼───────────────────────┐
│  REGIONAL MODELS        │   │  MPAS PRE-PROCESSING              │
│  GFS → RAP (13 km)      │   │  GEFS → MPAS init_atmosphere      │
│  RAP → HRRR (3 km)      │   │  Terrain/LULC/SST → Voronoi mesh  │
│  HRRR → HRRRe (6 mbr)   │   └───────────┬───────────────────────┘
└──────────────┬──────────┘               │
               │ ICs/BCs                  │
┌──────────────▼──────────┐   ┌───────────▼───────────────────────┐
│  WPS PRE-PROCESSING     │   │  WoFS-MPAS DA (15-min cycle)      │
│  geogrid/ungrib/metgrid │   │  Primary:  DART / EAKF            │
└──────────────┬──────────┘   │  Experimental: JEDI / MPAS-JEDI   │
               │              │  (all-sky radiances via CRTM)      │
┌──────────────▼──────────┐   └───────────┬───────────────────────┘
│  WoFS-WRF DA (15-min)   │               │
│  GSI-based EnKF         │   ┌───────────▼───────────────────────┐
│  MRMS + GOES L2 + BUFR  │   │  WoFS-MPAS Ensemble               │
└──────────────┬──────────┘   │  36 members · 3-km Voronoi mesh   │
               │              │  MPAS NetCDF output (5-min)        │
┌──────────────▼──────────┐   └───────────┬───────────────────────┘
│  WoFS-WRF Ensemble      │               │ (future/planned only)
│  36 members · 3-km WRF  │               │
│  WRFOUT NetCDF (5-min)  │               │
└──────────────┬──────────┘               │
               │                          ▼ ⚡ planned
               │              ┌───────────────────────────────────┐
               │              │  WoFSCast-MPAS Pipeline (future)  │
               │              │  TRAIN_PIPE_MPAS → GNN_MPAS       │
               │              │  INFERENCE_MPAS → PRODUCTS_MPAS   │
               │              └───────────────────────────────────┘
               └──────────────────────────┘
                              │
┌─────────────────────────────▼───────────────────────────────────┐
│  WoFSCast ML PIPELINE  (WoFS-WRF current; WoFS-MPAS future)     │
│                                                                 │
│  ── Current (WoFS-WRF) ──────────────────────────────────────── │
│  Training (offline):                                            │
│  WoFS-WRF archives → variable selection → icosahedral regrid    │
│  → normalize → input/target pairs → GNN training → weights      │
│                                                                 │
│  Inference (real-time):                                         │
│  Latest WoFS-WRF analysis → same preprocessing → GNN forward   │
│  pass → auto-regressive rollout (~30 sec on 1 GPU for 3-hr ens) │
│  → Tornado / hail / wind probability products → NWS / EmMgmt   │
│                                                                 │
│  ⚡ Planned — WoFSCast-MPAS (parallel future pipeline) ──────── │
│  WoFS-MPAS archives → TRAIN_PIPE_MPAS → GNN_MPAS model         │
│  WoFS-MPAS analysis  → INFERENCE_MPAS → PRODUCTS_MPAS          │
│  (WoFS-MPAS has NO current operational connection to WoFSCast)  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Key Upcoming Changes

| When | What | Effect on WoFSCast chain |
|------|------|--------------------------|
| ✅ Apr 2025 | GOES-19 → GOES East | Same L1b/L2 format; improved data quality |
| ✅ Dec 2025 | NOAA EAGLE operational (AIGFS/AIGEFS) | AI global models; shared ERA5 training; potential future BCs |
| ✅ Feb–Jul 2025 | ECMWF AIFS Single + ENS operational | Open-data global AI forecast; potential future BC source |
| **Oct 6, 2026** | **RRFS + REFS replace HRRR / RAP / HRRRe / NAM** | **WoFS BCs shift from HRRRe (WRF-ARW) → REFS (FV3); new training archive needed** |
| 2026–2027 | WoFS-RRFS development | FV3-driven WoFS; pre-processing pipeline update |
| ~2026 | WoFSCast HWT evaluation | Real-time operational path; forecaster testing |
| Planned | WoFSCast-MPAS Training Pipeline (TRAIN_PIPE_MPAS) | Parallel training pipeline using WoFS-MPAS archives + ERA5; experimental |
| Planned | WoFSCast-MPAS GNN Model (GNN_MPAS) | Separate GNN trained on MPAS data; parallel to current WoFS-WRF model |
| Planned | WoFSCast-MPAS Inference (INFERENCE_MPAS) | Real-time inference driven by WoFS-MPAS analysis (no current operational link) |
| Planned | WoFSCast-MPAS Products (PRODUCTS_MPAS) | Forecast products from the MPAS-based AI pipeline |
| Late 2020s | WoFSCast v2 (diffusion ensemble, 1-km) | Probabilistic generative model; sharper storm structures |
| ~Early 2030s | GeoXO constellation | Hyperspectral sounder from GEO orbit; transformative for WoFS DA |
| TBD | Phased Array Radar (PAR) | ~30-sec volume scans; faster radar DA cycling |

---

## Primary Sources

### WoFSCast & WoFS
- **WoFSCast paper (GRL 2024):** https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2024GL112383
- **WoFSCast GitHub (NOAA-NSSL):** https://github.com/NOAA-National-Severe-Storms-Laboratory/frdd-wofs-cast
- **NSSL WoFS overview:** https://www.nssl.noaa.gov/projects/wof/
- **WoFS real-time viewer:** https://wof.nssl.noaa.gov/
- **WoFS system configuration:** https://wof.nssl.noaa.gov/configuration.php

### Data Assimilation
- **GSI documentation (DTC):** https://dtcenter.org/community-code/gridpoint-statistical-interpolation-gsi
- **NCAR DART:** https://dart.ucar.edu/
- **DART for MPAS:** https://dart.ucar.edu/research/mpas/
- **JCSDA JEDI:** https://www.jcsda.org/jedi
- **MPAS-JEDI paper (Liu et al. 2022, GMD):** https://gmd.copernicus.org/articles/15/7859/2022/

### Models
- **GFS/FV3 (NOAA EMC):** https://www.emc.ncep.noaa.gov/emc/pages/numerical_forecast_systems/gfs.php
- **HRRR:** https://rapidrefresh.noaa.gov/hrrr/
- **RAP:** https://rapidrefresh.noaa.gov/
- **RAP/HRRR paper (Benjamin et al. 2016, MWR):** https://journals.ametsoc.org/view/journals/mwre/144/4/mwr-d-15-0242.1.xml
- **MPAS project (NCAR/LANL):** https://mpas-dev.github.io/
- **ECMWF AIFS:** https://www.ecmwf.int/en/forecasts/documentation-and-support/aifs
- **ERA5 (Copernicus CDS):** https://cds.climate.copernicus.eu/datasets/reanalysis-era5-pressure-levels
- **GraphCast paper (DeepMind, Science 2023):** https://www.science.org/doi/10.1126/science.adi2336

### Observing Systems
- **NEXRAD Radar Operations Center:** https://www.roc.noaa.gov/
- **NSSL MRMS:** https://www.nssl.noaa.gov/projects/mrms/
- **GOES-R series:** https://www.goes-r.gov/
- **GOES-19 transition:** https://www.goes-r.gov/users/transitionToOperations19.html
- **COSMIC-2 GNSS-RO (UCAR):** https://www.cosmic.ucar.edu/
- **NOAA AMDAR aircraft obs:** https://amdar.noaa.gov/

### Open Data Access
- **NEXRAD on AWS:** https://registry.opendata.aws/noaa-nexrad/
- **GOES-16/18 on AWS:** https://registry.opendata.aws/noaa-goes/
- **MRMS QPE on AWS:** https://registry.opendata.aws/noaa-mrms-qpe/
- **GFS on AWS:** https://registry.opendata.aws/noaa-gfs-bdp-pds/
- **HRRR on AWS:** https://registry.opendata.aws/noaa-hrrr-pds/
- **ERA5 on AWS:** https://registry.opendata.aws/ecmwf-era5/
- **ECMWF AIFS open data:** https://www.ecmwf.int/en/forecasts/dataset/aifs-machine-learning-data
- **AIGFS on AWS:** https://registry.opendata.aws/noaa-nws-graphcastgfs-pds/
- **RRFS prototype on AWS:** https://registry.opendata.aws/noaa-rrfs/
- **NOMADS (all NCEP models):** https://nomads.ncep.noaa.gov/

### Future Systems
- **RRFS/REFS Service Change Notice 26-48:** https://www.weather.gov/media/notification/pdf_2026/scn26-48_RRFS_and_REFS_Implementation.pdf
- **NOAA EPIC Project EAGLE:** https://epic.noaa.gov/noaa-project-eagle-to-accelerate-ai-weather-prediction-advances-for-the-united-states/
- **NOAA GeoXO program:** https://www.nesdis.noaa.gov/next-generation/geoxo
- **NSSL Phased Array Radar:** https://www.nssl.noaa.gov/projects/par/
- **GenCast paper (DeepMind, Nature 2024):** https://www.nature.com/articles/s41586-024-08252-9

---

*Created: August 2026 · NOAA/NSSL WoFSCast Data Pipeline reference*
