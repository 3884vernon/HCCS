# How to push this bundle to the HCCS repository

There's no GitHub connector in this session, so here are two ways to get these files into your existing **HCCS** repo. Pick one.

Your repo (already created, shared with 1 collaborator):
```
https://github.com/<your-username>/HCCS
```
Replace `<your-username>` below with your actual GitHub username.

---

## Option A — Paste this into Claude Code (recommended)

Open Claude Code in the folder where this `HCCS` bundle lives (the same folder that contains `ssm-library-analyzer-v10_2.html`), then paste the prompt below verbatim:

> Set up this folder as the local clone of my existing GitHub repo `https://github.com/<your-username>/HCCS` and push everything.
>
> Steps:
> 1. Run `git init` if this isn't already a git repo, then set `git remote add origin https://github.com/<your-username>/HCCS.git` (or update the remote if origin already exists).
> 2. Fetch the remote so we don't clobber anything: `git fetch origin`. If `main` already has commits, rebase our local files on top of it rather than force-pushing.
> 3. Stage all the files in this folder, respecting the existing `.gitignore` (do NOT commit anything under `data/`, or any `.fastq`, `.csv`, `.pdf`).
> 4. Commit with the message: `chore: initialize repository with SSM analyzer v10.2 and project docs`.
> 5. Protect against accidents: confirm the working tree matches what's staged and show me `git status` and `git log --oneline` before pushing.
> 6. Push to `origin main`. If the remote rejects because it has commits I don't have, pull with `--rebase`, resolve, and push again — never force-push.
> 7. After the push, remind me to enable branch protection on `main` in GitHub settings (require a pull request + 1 review, per our CONTRIBUTING.md), since that has to be set in the GitHub web UI.

Claude Code will need your git credentials configured (a GitHub Personal Access Token or SSH key). If it prompts, follow its instructions.

---

## Option B — Run it yourself in a terminal

From inside the `HCCS` folder:

```bash
# 1. Initialize and point at your existing repo
git init
git branch -M main
git remote add origin https://github.com/<your-username>/HCCS.git

# 2. See what the remote already has (it may be empty or have a README)
git fetch origin

# 3. Stage everything (.gitignore keeps data/ and raw files out)
git add .
git status                       # sanity-check: no .fastq / .csv / data/ files listed

# 4. Commit
git commit -m "chore: initialize repository with SSM analyzer v10.2 and project docs"

# 5a. If the remote is EMPTY:
git push -u origin main

# 5b. If the remote already has commits (e.g. an auto-created README):
git pull --rebase origin main    # replay your commit on top of theirs
# resolve any conflicts, then:
git push -u origin main
```

> ⚠️ Do **not** use `git push --force` — your collaborator may have work on the remote.

---

## After the first push: turn on the rules

The commit rules in `CONTRIBUTING.md` only bite if branch protection is enabled. Do this once in the GitHub web UI:

1. Go to **`HCCS` → Settings → Branches → Add branch ruleset** (or "Add rule") for `main`.
2. Enable **Require a pull request before merging** and **Require approvals: 1**.
3. Enable **Do not allow bypassing the above settings** if you want it enforced for admins too.
4. Save.

Now every change goes through a PR reviewed by the other collaborator, exactly as documented.

---

## What's in this bundle

```
HCCS/
├── ssm-library-analyzer-v10_2.html   ← the analyzer (renamed from ssm-analyzer-v10_2.html)
├── README.md
├── CONTRIBUTING.md                    ← the commit rules
├── CHANGELOG.md
├── PUSH_TO_GITHUB.md                  ← this file (you can delete it after pushing)
├── .gitignore
├── docs/WORKFLOW.md
├── docs/ARCHITECTURE.md
├── data/.gitkeep                      ← empty placeholder; put local FASTQ here (git-ignored)
└── .github/
    ├── PULL_REQUEST_TEMPLATE.md
    └── ISSUE_TEMPLATE/{bug_report,feature_request}.md
```
