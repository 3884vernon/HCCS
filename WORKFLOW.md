# Workflow: connecting FACS-sorted variants to sequencing results

This is the core purpose of the project. An SSM (Site-Saturation Mutagenesis) library is sorted by FACS into activity bins, each bin is sequenced, and the analyzer links the two so we can score each variant's activity.

## The pipeline

```
 1. SSM library          2. FACS sort              3. Barcoded prep &          4. Analyzer
    (NNK/NNS at              into Q1–Q4               sequencing                   (this repo)
     target sites)           by ΔF/F               each quadrant → its own
                             (+ cells sorted)        UDI-barcoded FASTQ

  variant pool   ───►   Q1  low  ΔF/F   ───►   UDI-A → library_A.fastq  ───►   UDI → quadrant map
                        Q2               ───►   UDI-B → library_B.fastq         per-variant counts
                        Q3               ───►   UDI-C → library_C.fastq         per gate
                        Q4  high ΔF/F    ───►   UDI-D → library_D.fastq         log2 enrichment
```

## Step by step

### 1. Sort by FACS
Cells expressing the variant library are sorted into up to four quadrants (**Q1–Q4**) defined by a **ΔF/F** signal window. For each quadrant you record:
- the **ΔF/F gate range** (e.g. `0–25%`, `25–50%`), and
- the **number of cells sorted**.

### 2. Barcode and sequence each quadrant
Each sorted quadrant is prepared as its own library and tagged with a **UDI (Unique Dual Index)**. Sequencing produces one FASTQ file per quadrant. The UDI is the key that ties a physical sort gate to a specific set of reads.

### 3. Map UDIs to quadrants in the analyzer
On the **Setup** tab you enter, for each UDI:
- the FASTQ file,
- the quadrant (Q1–Q4) it came from,
- the ΔF/F gate range, and
- the cells-sorted count.

This mapping is what makes the FACS ↔ sequencing connection explicit.

### 4. Count variants per gate
The analyzer parses each FASTQ, identifies the variant codon(s) at the target site(s), and tallies molecule counts per variant within each quadrant. It also reports **library balance** — per-codon and per-amino-acid coverage against the NNK/NNS expectation — so you can judge whether the library is even enough to trust downstream scores.

### 5. Compute cross-quadrant enrichment (Sort-Seq ΔF/F)
For a given variant, enrichment between two quadrants is:

```
enrichment(Qi → Qj) = log2( total_molecules(variant, Qj) / total_molecules(variant, Qi) )
```

normalized by each quadrant's total molecules. When numeric **ΔF/F midpoints** can be parsed from the gate strings (e.g. `0-25%` → midpoint 12.5%), variants are ordered by inferred activity; otherwise the tool falls back to per-quadrant frequencies. A variant enriched in the high-ΔF/F quadrant relative to the low one is inferred to be more active.

### 6. Export
The **Export** tab produces data exports and a formatted **PDF report** capturing the mapping, balance scores, and enrichment table for the run.

## Inputs & outputs at a glance

| Stage    | Input                                             | Output                                  |
|----------|---------------------------------------------------|-----------------------------------------|
| Setup    | FASTQ per UDI + quadrant / ΔF/F gate / cell count | validated UDI → quadrant map            |
| Results  | mapped libraries                                  | coverage, balance score, enrichment     |
| Export   | computed results                                  | data export + PDF report                |

## Data handling note
Raw FASTQ and exported tables are **git-ignored** (see [`../.gitignore`](../.gitignore)). Keep them in `data/` locally or in shared lab storage. The repository tracks the tool and its documentation, not experimental data.
