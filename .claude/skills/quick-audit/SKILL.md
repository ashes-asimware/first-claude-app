---
name: quick-audit
description: Quick read-only health check of this repo: git state, JSON-ignore rule, and consistency of the systems.*.json config files. Use when the user asks to audit or sanity-check the repo.
---

# Quick audit

Run a fast, read-only audit of this repository and report the findings. Do not modify, stage, or commit anything.

## Checks

1. **Git state**
   - Run `git status --short` and `git branch --show-current`.
   - Report the current branch and any untracked or modified files (e.g. screenshots in `images/`, `.claude/skills/`).
   - Warn if the current branch is `main`, since work should happen on feature branches merged via PR.

2. **No JSON tracked**
   - Run `git ls-files "*.json"`.
   - Any output is a failure: `*.json` is git-ignored and the `systems.*.json` files must never be committed. Flag any `git add -f` evidence.
   - Confirm `.gitignore` still contains the `*.json` rule.

3. **Systems config consistency** (local files only; skip with a note if absent)
   - For each of `systems.development.json`, `systems.staging.json`, `systems.production.json` in the project root:
     - Parses as valid JSON.
     - Has a top-level `systems` array whose entries each have `id`, `name`, `description`, `environment`, `owner`, `url`, `status`, `tags`.
     - Every entry's `environment` matches the filename.
     - `id` values are unique within the file.
     - Has a `settings.ocr_app` block.
   - Across files, flag `ocr_app` keys that exist in one environment but not the others.

4. **Docs vs. reality**
   - Compare `.claude/CLAUDE.md` "Status" against the repo: if source code, a build system, or tests now exist, note that CLAUDE.md is stale.

## Output

Give a short report: one line per check with PASS / WARN / FAIL, followed by details only for non-passing checks. End with a suggested next step if anything failed. Keep it under ~20 lines.
