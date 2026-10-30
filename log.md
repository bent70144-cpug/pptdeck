# DeckCraft — Project Change Log (`log.md`)

Central, continuously updated record of all development activities, modifications, and significant changes for this project. Newest entries appear at the top. Every change must be logged here and synchronized to the GitHub repository (`bent70144-cpug/pptdeck`, branch `main`).

---

## Project Overview

- **Project:** DeckCraft — an open-source, modern presentation and slide designer built in pure Rust (upstream: `storytold/deckcraft`).
- **This repository:** `bent70144-cpug/pptdeck` — mirror of the upstream `main` branch plus local development changes.
- **License:** MIT OR Apache-2.0 (dual license, inherited from upstream).
- **Status:** Early development (upstream project status).

## Initial Project State (as of bootstrap)

- Cargo workspace with 30+ crates, including:
  - **Core:** `engine` (command layer, slide/shape/table/chart/animation commands), `model` (document model), `format` (rich text/parsing), `text` (text layout/bidi/shaping), `geom` (geometry + preset shapes), `color`, `anim` (animations/transitions/timelines), `render` (CPU renderer), `metafile` (EMF/WMF support).
  - **File format:** `pptx` (read/write of `.pptx` via OPC), `pdf` (PDF export).
  - **Media:** `h264`, `hevc`, `av1`, `vp9` (video decoders), `opus` (audio), `media` (playback pipeline), `isobmff`, `matroska`, `ogg` (containers).
  - **UI:** `ui-egui` (ribbon UI, canvas, dialogs, panels, slideshow), `fonts` (font database/fallback/shaping).
  - **Automation:** `mcp` (Model Context Protocol server), CLI app `deckcraft-cli`, control server in the desktop app.
- **Applications:** `apps/deckcraft` (native desktop), `apps/deckcraft-web` (WASM/browser via Trunk), `apps/deckcraft-cli` (command line + automation), `xtask` (build/packaging tooling).
- **Docs:** `docs/` with architecture, control protocol, parity checklists (UI/file-format/animation/slideshow/hardware/localization), roadmap.
- **Packaging:** macOS, Windows (WiX), Linux (Flatpak/nfpm), FreeBSD, Web.

---

#### [2026-10-30 16:20] — Install Matt Pocock agent skills package (38 skills) via `skills` CLI

- **Type:** Configuration
- **Description:** Installed the full agent-skills package from `github.com/mattpocock/skills` using the Vercel Labs `skills` CLI (npm package `skills` v1.7.4) with non-interactive flags: `npx skills add mattpocock/skills -s '*' -y`. All 38 skills were installed at project level into the canonical `.agents/skills/` store (universal format readable by Amp, Antigravity, Cline, Codex and 17+ other agents; none symlinked elsewhere since no other agent directories exist in this environment), and a `skills-lock.json` lockfile (v1, per-skill source path + hash) was created at the repo root. Skills include engineering workflows (`code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `implement`, `implement-spec`, `tdd`, `to-spec`, `to-tickets`, `triage`, `pr`, `research`, `retro`, `prototype`, `improve-codebase-architecture`, `grill-with-docs`, `setup-ts-deep-modules`, `setup-pre-commit`), interaction/meta skills (`ask-matt`, `grilling`, `grill-me`, `wait-what`, `teach`, `handoff`, `claude-handoff`, `wayfinder`, `wizard`, `loop-me`, `chief-of-staff`), writing skills (`writing-beats`, `writing-fragments`, `writing-shape`, `writing-for-agents`, `to-questionnaire`), and utilities (`git-guardrails-claude-code`, `migrate-to-shoehorn`, `scaffold-exercises`, `setup-matt-pocock-skills`). Note: the first attempt without flags hung on the CLI's interactive skill picker and was aborted; the flag-based invocation completed cleanly.
- **Reason:** User requested `npx skills add mattpocock/skills`; these skills provide reusable, documented agent workflows (spec-driven implementation, TDD, code review, debugging diagnosis, technical writing) for future development on this repository.
- **Files Affected:**
  - `.agents/skills/` (created — 38 skill directories, each containing `SKILL.md` plus optional `agents/openai.yaml`, `scripts/`, and reference docs)
  - `skills-lock.json` (created — lockfile enabling `skills experimental_install` to restore the exact set)
- **Impact:** No runtime/build behavior of DeckCraft itself is affected; the repository gains an in-repo agent-skills library that AI coding agents can discover and use during development. Repository file count grows by ~100 files.
- **Testing:** Verified 38/38 skill directories contain a `SKILL.md`; parsed `skills-lock.json` (valid JSON, v1 schema, sources pinned to `mattpocock/skills`); `npx skills ls` lists all installed skills ("Source: mattpocock/skills"); `git status` shows only the two expected additions.
- **Status:** Completed
- **GitHub Sync:** Pending (push to be executed immediately after this entry is committed)
- **Commit:** (this change's commit — hash to be recorded in the follow-up log update once pushed and verified)

---

#### [2026-10-30 16:01] — Establish project change log (`log.md`)

- **Type:** Documentation
- **Description:** Created this `log.md` in the project root as the central change-log file, including project overview, initial architecture state, and bootstrap entries for the clone and the GitHub mirror push. Defines the standard entry format and the dual-sync (local + GitHub) workflow rule for all future changes.
- **Reason:** Mandatory project-wide rule: all development activity must be documented, traceable, and synchronized between the local environment and GitHub.
- **Files Affected:**
  - `log.md` (created)
- **Impact:** No runtime behavior change; adds a documentation and process baseline for the repository.
- **Testing:** Verified file renders as valid Markdown; verified all bootstrap facts (commit hash, branch, remote URL) against `git log` and `git ls-remote` output.
- **Status:** Completed
- **GitHub Sync:** Synced (verified via `git ls-remote`: `main` = `343ab6dc`)
- **Commit:** `343ab6dc` (introduces `log.md`; hash recorded in follow-up entry update)

---

#### [2026-10-30 15:55] — Mirror `main` branch to GitHub repository `bent70144-cpug/pptdeck`

- **Type:** Configuration
- **Description:** Pushed the complete local `main` branch (full commit history, HEAD at upstream commit `3a4e30e1` "UPD.1: honor pretend versions and restore dev-testkit checks (#1562)", ~12 MB of Git objects) to the empty GitHub repository `bent70144-cpug/pptdeck`. Remote added locally as `myrepo`. First push attempt was rejected by GitHub because the repository contains GitHub Actions workflow files (`.github/workflows/*.yml` — 7 files: `ci.yml`, `freebsd.yml`, `nix.yml`, `packaging-lint.yml`, `release.yml`, `sonarcloud.yml`, `windows-arm64.yml`) and the access token lacked the Workflows permission; after the token permission was updated to Workflows: Read and write, the push succeeded.
- **Reason:** User requested the cloned DeckCraft code be sent to their own GitHub repository for safekeeping/development.
- **Files Affected:**
  - Remote repository `bent70144-cpug/pptdeck` (branch `main` created; no local source files changed)
- **Impact:** The full upstream history is now available under the user's GitHub account as the project's synchronization target. All future local changes will be committed and pushed here.
- **Testing:** `git ls-remote https://github.com/bent70144-cpug/pptdeck.git` verified `refs/heads/main` = `3a4e30e1`, matching local `main` HEAD exactly.
- **Status:** Completed
- **GitHub Sync:** Synced (verified via `git ls-remote`)
- **Commit:** `3a4e30e1` (HEAD pushed)

---

#### [2026-10-30 15:41] — Clone upstream repository `storytold/deckcraft`

- **Type:** Configuration
- **Description:** Cloned `https://github.com/storytold/deckcraft.git` (~26 MB working tree) into the project root. Checkout is branch `main` at commit `3a4e30e1`; all upstream remote branches fetched as remote-tracking refs (e.g., `a-av1/*`, `a-hevc/*`). Only the `main` branch exists locally.
- **Reason:** Initial bootstrap of the local development environment for this project.
- **Files Affected:**
  - Entire repository tree (clone; no source modifications made at this step)
- **Impact:** Establishes the local baseline codebase (Rust workspace, 30+ crates, 3 applications) on which all subsequent development happens.
- **Testing:** Verified clone integrity via `git log --oneline -5` (expected recent upstream commits present), `git status` (clean tree), and remote branch listing (`git branch -a`).
- **Status:** Completed
- **GitHub Sync:** Synced (code state included in the mirror push above; verified)
- **Commit:** `3a4e30e1`
