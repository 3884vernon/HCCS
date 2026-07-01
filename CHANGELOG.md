# Changelog

All notable changes to the SSM Library Balance Analyzer are recorded here. The analyzer version lives in its filename (`ssm-library-analyzer-vMAJOR_MINOR.html`). See [CONTRIBUTING.md](CONTRIBUTING.md#versioning-the-analyzer) for the bump rules.

## v10.2 — initial repository import

- First version committed to the HCCS repository.
- Setup / Results / Export tab workflow.
- FASTQ library upload and per-codon / per-amino-acid coverage with library balance score.
- UDI → FACS quadrant (Q1–Q4) mapping with ΔF/F gate ranges and cells-sorted counts.
- Cross-quadrant Sort-Seq ΔF/F enrichment: `log2(ratio of total molecules)` per variant, ordered by ΔF/F midpoints parsed from gate strings.
- Data export and PDF report generation.

_Add new entries above this line when you bump the version._
