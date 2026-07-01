# Contributing to HCCS

This is a small, two-person project. These rules keep history readable and prevent the shared analyzer file from getting into a broken or conflicting state. Please read before your first commit.

## Golden rules

1. **Never commit directly to `main`.** Always work on a branch and open a pull request.
2. **One logical change per commit.** If you can't describe a commit in one line, split it.
3. **Never commit raw sequencing data or large binaries.** FASTQ files, `.csv` exports, and sample data belong in `data/` (which is git-ignored) or in shared storage — not in the repo.
4. **Test the HTML file in a browser before pushing.** Open it, run a small sample through Setup → Results → Export, and confirm nothing throws in the console.

## Branching

Branch off `main` using this naming scheme:

```
feature/<short-description>    # new capability (e.g. feature/per-codon-heatmap)
fix/<short-description>        # bug fix (e.g. fix/udi-quadrant-parse)
docs/<short-description>       # documentation only
chore/<short-description>      # tooling, cleanup, versioning
```

Keep branch names lowercase with hyphens. Delete the branch after it merges.

## Commit message format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short summary in the imperative mood>

<optional body — what & why, not how>

<optional footer — issue references, breaking changes>
```

**Types**

| Type       | Use for                                                        |
|------------|---------------------------------------------------------------|
| `feat`     | A new feature in the analyzer                                  |
| `fix`      | A bug fix                                                      |
| `docs`     | Documentation only                                             |
| `refactor` | Code change that neither fixes a bug nor adds a feature        |
| `perf`     | Performance improvement                                        |
| `style`    | Formatting / whitespace, no logic change                      |
| `chore`    | Tooling, versioning, housekeeping                             |

**Scope** (optional) names the affected area, e.g. `setup`, `results`, `export`, `enrichment`, `udi`, `pdf`.

**Examples**

```
feat(enrichment): add cross-quadrant log2 ΔF/F scoring
fix(udi): correct Q1–Q4 mapping when a UDI is reused across plates
docs(workflow): document FACS gate → UDI → library linkage
chore: bump analyzer to v10_2
```

**Rules**

- Summary line ≤ 72 characters, imperative mood ("add", not "added"/"adds"), no trailing period.
- Explain *why* in the body when the reason isn't obvious.
- Reference issues in the footer: `Closes #12`.

## Pull request workflow

1. Push your branch and open a PR against `main`. Fill in the PR template.
2. **The other collaborator reviews and approves before merge.** No self-merging of non-trivial changes. (Typo/doc fixes may be merged directly if the other person is unavailable — note this in the PR.)
3. Prefer **"Squash and merge"** so `main` keeps one clean commit per PR.
4. Ensure the analyzer opens and runs in a browser before approving.

## Versioning the analyzer

The analyzer file is versioned in its **filename**: `ssm-library-analyzer-vMAJOR_MINOR.html`.

- **Minor bump** (`v10_2` → `v10_3`): new features or fixes that don't change how existing runs are interpreted.
- **Major bump** (`v10_x` → `v11_0`): changes that alter results, exports, or the required input format.

When you bump the version:

1. Rename the file to the new version.
2. Add an entry at the top of [`CHANGELOG.md`](CHANGELOG.md).
3. Update any references in `README.md`.

Keeping the version in the filename lets collaborators tell at a glance which build produced a given report.

## Reporting problems

Open an issue using the appropriate template in `.github/ISSUE_TEMPLATE/`. For anything involving specific sequencing data, describe the input rather than attaching raw files.
