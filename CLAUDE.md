# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status
This repository is currently a placeholder: the tracked files are `README.md` (`# placeholder`), `.gitignore`, and this file.
There is no source code, build system, test suite, or lint configuration yet.

## Local systems config
Per-environment inventories of example systems live in the project root as `systems.development.json`, `systems.staging.json`, and `systems.production.json`.

- Each file has a top-level `systems` array. Each entry has `id`, `name`, `description`, `environment`, `owner`, `url`, `status`, and `tags`.
- Each file also has a top-level `settings.ocr_app` block configuring a Python app that parses text from images (Tesseract engine, languages, preprocessing, input/output dirs, supported formats, size limit, workers, minimum confidence, debug images, logging). Values vary by environment (e.g. more workers, higher DPI and confidence, and `WARNING` logging in production). The key names are the author's choice, not taken from an existing app.
- In each file, every entry's `environment` matches the filename. The values (owners, `example.com` URLs) are made-up examples.
- All `*.json` files are git-ignored (`*.json` in `.gitignore`), so these files are local-only and must never be committed. Don't use `git add -f` on them. The pattern is deliberately broad: if the project later needs a tracked JSON file (e.g. `package.json`), add a negation rule such as `!package.json` to `.gitignore`.

## Git workflow
Work happens on feature branches merged into `main` via pull request. The user merges PRs manually on GitHub after checks and the Copilot review run, so don't merge them.
