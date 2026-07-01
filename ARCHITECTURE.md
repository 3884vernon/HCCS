# Architecture

The analyzer is a **single self-contained HTML file** — `ssm-library-analyzer-v10_2.html`. Markup, CSS, and JavaScript all live in that one file, with fonts pulled from Google Fonts. There is no build step, no bundler, and no server: open the file and it runs.

## Why a single file
- Trivial to share (email, drive, or open straight from the repo).
- Runs offline; sequence data never leaves the machine.
- Nothing to install — important for collaborators who aren't developers.

The tradeoff is that the file is large and edits touch one document, so coordinate changes through PRs (see [CONTRIBUTING.md](../CONTRIBUTING.md)) rather than editing `main` in parallel.

## Internal structure
The UI is organized as three top-level tabs, each a `top-panel`:

- **`panel-setup`** — FASTQ upload plus the UDI → quadrant / ΔF/F-gate mapping (`udi-quadrant-panel`).
- **`panel-results`** — per-codon / per-amino-acid coverage, library balance, and the cross-quadrant enrichment section (`quadrant-enrichment-section` / `quadrant-enrichment-content`).
- **`panel-export`** — data export and PDF report configuration (`pdf-config-panel`).

Results and Export tabs stay disabled until Setup has valid inputs.

## Key logic areas (search terms inside the file)
| Concern                         | Look for                                             |
|---------------------------------|------------------------------------------------------|
| UDI → quadrant mapping          | `dffGate`, `cellsSorted`, `byQ`, `distinctUDIs`      |
| Variant/codon calling           | `codon`, `NNK`, `barcode`                            |
| Cross-quadrant enrichment       | `canSortByDff`, `midpoint`, `log2`, `total_molecules`|
| Export / PDF                    | `pdf-config-panel`, PDF report preview               |

## Editing guidance
- Keep everything in the one HTML file unless we deliberately decide to split the project.
- When you change analyzer behavior, bump the version in the filename and update [CHANGELOG.md](../CHANGELOG.md).
- Test in a browser (DevTools console open) before opening a PR — there is no automated test suite.
