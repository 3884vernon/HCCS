# HCCS — SSM Library Balance Analyzer

Tooling for the HCCS project. The goal of this repository is to **connect FACS-sorted variant data to sequencing results**: each FACS sort gate (quadrant Q1–Q4, defined by a ΔF/F range and a cell count) is linked to its sequencing library through UDI barcodes, so we can compute per-variant enrichment across sort gates.

The core tool is a single self-contained HTML application — **`ssm-library-analyzer-v10_2.html`** — that runs entirely in the browser (no server, no install). Open it in any modern browser and work through the three tabs: **Setup → Results → Export**.

## What the analyzer does

- **Upload FASTQ libraries.** Load one or more sequenced libraries from a Site-Saturation-Mutagenesis (SSM) campaign.
- **Map sort gates to libraries.** Assign each UDI (unique dual index) to a FACS quadrant (Q1–Q4) plus its ΔF/F gate range and number of cells sorted.
- **Score library balance.** Per-codon and per-amino-acid coverage, NNK/NNS expectations, and a single balance score summarizing how evenly the library is represented.
- **Cross-quadrant ΔF/F enrichment (Sort-Seq).** Enrichment between two quadrants is computed as `log2(ratio of total molecules)` per variant, using the ΔF/F midpoints parsed from each gate to order variants by inferred activity.
- **Export.** Generate data exports and a formatted PDF report of the run.

## How the FACS ↔ sequencing link works

See [`docs/WORKFLOW.md`](docs/WORKFLOW.md) for the full pipeline. In short:

```
FACS sort           Sequencing              Analyzer
─────────           ──────────              ────────
Q1–Q4 gates    →    UDI-barcoded      →     UDI → quadrant map
(ΔF/F range,        FASTQ libraries         → per-variant counts
 cells sorted)                              → log2 enrichment across gates
```

## Quick start

1. Download or clone this repository.
2. Open `ssm-library-analyzer-v10_2.html` in Chrome, Safari, Edge, or Firefox.
3. On the **Setup** tab, upload your FASTQ files and enter the UDI → quadrant / ΔF/F gate mapping.
4. Review **Results**, then generate an export or PDF report on the **Export** tab.

All processing happens locally in your browser. No sequence data leaves your machine.

## Repository layout

```
HCCS/
├── ssm-library-analyzer-v10_2.html   # the analyzer app (main deliverable)
├── README.md                          # this file
├── CONTRIBUTING.md                    # commit & contribution rules — read before pushing
├── CHANGELOG.md                       # notable changes per version
├── .gitignore
├── docs/
│   ├── WORKFLOW.md                    # FACS ↔ sequencing pipeline, in detail
│   └── ARCHITECTURE.md                # how the HTML app is structured
├── data/                              # (git-ignored) local FASTQ / sample data
└── .github/
    ├── PULL_REQUEST_TEMPLATE.md
    └── ISSUE_TEMPLATE/
        ├── bug_report.md
        └── feature_request.md
```

## Contributing

This is a two-person project. Please read [`CONTRIBUTING.md`](CONTRIBUTING.md) before making changes — it covers branch naming, commit-message format, the pull-request review rule, and how we version the analyzer file.

## Versioning

The analyzer is versioned in its filename (`v10_2` = version 10.2). See [CHANGELOG.md](CHANGELOG.md) and [`CONTRIBUTING.md`](CONTRIBUTING.md#versioning-the-analyzer) for how versions are bumped.
