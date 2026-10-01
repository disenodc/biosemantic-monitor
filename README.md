# 🧬 BioSemantic Monitor v4.1

A single-file, browser-based dashboard for exploring biodiversity occurrence data on an interactive map and enriching it with semantic sources (taxonomy, species interactions, taxonomic literature). It combines live **GBIF** occurrence data with **Catalogue of Life (COL)**, **GloBI**, and **Plazi** enrichment, and presents the results as a map, a force-directed knowledge graph, charts, and metrics.

No build step, no backend, no API keys: everything runs client-side in one HTML file.

---

## Features

### 🗺️ Map view
- **Live GBIF occurrences** drawn as markers (up to 500 on the map at a time), color-coded by kingdom. Species in the built-in IUCN list are highlighted in red.
- **GBIF density tile layer** that follows the active country and taxon filters.
- **Basemaps:** dark, satellite, light (Esri) and OpenStreetMap.
- **Filters:** year range, country (18 preset countries), and taxon group (Animals, Plants, Fungi, Birds).
- **Bulk Load:** pages through the GBIF search API in batches of 300 records (5 concurrent requests). Choose 3,000 / 10,000 / 50,000 / 100,000 / All available, with a progress bar and a stop button. GBIF offsets are capped at 200,000 records per load.
- **Click-to-query zone:** click anywhere on the map to fetch up to 100 occurrences within a ~0.05° box around the point. Use *Restore* to return to the full dataset.
- **Geographic Explorer:** drill down World → Continent → Country; selecting a country zooms the map and applies the filter.
- **Temporal Dataset Navigator:** a 1900–2025 slider that lists loaded records within ±5 years, grouped by institution, with links to the originating GBIF datasets.
- **Records panel:** browsable list (first 100 records) with a per-record detail view (taxonomy, country, year, institution, coordinates, dataset link).

### 🧠 Analysis view
- **Knowledge Graph (D3 force layout):** country-centric graph of countries, species and related entities. Switch between *All countries* and a random/selected country subgraph.
- **Semantic enrichment** for the selected species:
  - **🔗 GloBI**: species interactions (eats, preyed upon, parasite of, pollinates, …) added as nodes and edges.
  - **📜 Plazi**: taxonomic treatments from TreatmentBank.
  - **📚 COL**: taxonomic data from the Catalogue of Life (via the ChecklistBank API).
- **Charts (Chart.js):** records by species, country, year (latest 30 years), and institution.
- **Metrics:** totals for records, species, countries, institutions and threatened species, plus node-type cluster cards.

### 📡 Data Sources registry
The sidebar lists eight source categories (Taxonomy, Interactions, Traits, Literature, Genomics, Conservation, Repositories, Ontologies) with on/off toggles.

| Status | Sources |
|---|---|
| **available** | GBIF, Catalogue of Life, GloBI, Plazi TreatmentBank, IUCN (local lookup) |
| **demo** (placeholder, no API wired up yet) | TDWG, NCBI Taxonomy, Web of Life, Interaction Web DB, TRY, Amniote DB, EOL, BHL, Europe PMC, BOLD, GenBank, UniProt, CITES, DataONE, PANGAEA, OBO Foundry, BioPortal |

---

## Getting started

1. Save the file as `index.html`.
2. Open it in a modern browser (Chrome, Firefox, Edge, Safari). An internet connection is required.

Some browsers restrict cross-origin requests from `file://` pages. If APIs fail to load, serve the folder locally instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Usage

1. **Pick filters** in the left sidebar (country, taxon group, year range) and click **Apply Filters**.
2. **Load more data** with **Bulk Load → Start**. Choose a limit first if you don't want everything.
3. **Explore the map:** click markers for popups, click empty map for a zone query, use the Geographic Explorer to jump between regions, and open **📅 Time** for the temporal navigator.
4. Switch to the **🧠 Analysis** tab for the knowledge graph, charts and metrics.
5. In the Knowledge Graph, select a species node, then use **GloBI / Plazi / COL** to enrich it.

## Dependencies (loaded from CDNs)

| Library | Version | Purpose |
|---|---|---|
| [Leaflet](https://leafletjs.com/) | 1.9.4 | Map |
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Charts |
| [D3](https://d3js.org/) | v7 | Knowledge graph |

## External services

| Service | Used for |
|---|---|
| [GBIF API](https://api.gbif.org) | Occurrence search and density tiles |
| [ChecklistBank / COL](https://api.checklistbank.org) | Taxonomic enrichment |
| [GloBI](https://api.globalbioticinteractions.org) | Species interactions |
| [Plazi TreatmentBank](https://tb.plazi.org) | Taxonomic treatments |
| [Esri ArcGIS](https://server.arcgisonline.com) / [OpenStreetMap](https://tile.openstreetmap.org) | Basemap tiles |

## Known limitations

- **Demo fallback:** if GloBI or Plazi can't be reached (or return nothing), the app shows clearly labeled *demo* data. Demo GloBI interactions exist only for a handful of genera (e.g. *Panthera*, *Pan*, *Loxodonta*, *Gorilla*, *Ursus*, *Harpia*, *Tremarctos*).
- **IUCN status** comes from a small hard-coded list of 14 species in the source, not the live IUCN Red List API.
- **Source toggles** currently track which sources are "active" (and update the counters) but do not yet change which APIs are queried.
- **Source status badges** marked `demo` are placeholders for future integrations.
- **Performance:** loading very large datasets keeps all records in browser memory, and only the first 500 markers and 100 list entries are rendered. Use filters or a lower limit on slower machines.
- **Rate limits and CORS:** public APIs may throttle heavy use or block cross-origin requests; results can vary by network.

## Customizing

All configuration lives at the top of the `<script>` block:

- `DATA_SOURCES`: the source registry (add a source or change its status/API URL).
- `IUCN`: threatened-species lookup (scientific name → status code and common name).
- `CBOUNDS` and the `<select id="sel-country">` options: country bounding boxes and the country list.
- `GEO_HIERARCHY`: the continent → country tree in the Geographic Explorer.
- `BASEMAPS`: tile providers.

## Data attribution

Occurrence data © GBIF and its data publishers. Taxonomy © Catalogue of Life. Interactions from Global Biotic Interactions (GloBI). Treatments from Plazi. Please cite the original datasets (linked from each record) when reusing results.