<!-- markdownlint-disable MD013 MD024 -->
# Changelog

All notable changes to the `toolbelt` plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.0] - 2026-04-17

### Changed

- `deep-research`, `gh-create-issue`, `gh-fix-pr-review`: add
  `disable-model-invocation: true` — these skills all have side effects or spend significant
  tokens (200K-700K for research, GitHub mutations for the two `gh-*` skills), so Claude no longer
  auto-loads them. Invoke explicitly via `/deep-research`, `/gh-create-issue`,
  `/gh-fix-pr-review`.
- `gh-create-issue`: add `allowed-tools: Bash(gh:*) Read Write WebFetch` and
  `argument-hint: "[title-or-plan-text]"` — pre-approves the `gh` CLI calls the skill makes and
  improves autocomplete.
- `deep-research`: switch `allowed-tools` to the space-separated form documented in the Claude
  Code Skills reference.

### Added

- `code-audit` skill — read-only deep audit of a repository or subtree producing a severity-ranked
  Markdown report (`audit-<slug>-<YYYYMMDD>.md`) at the repo root. Trail-of-Bits-style findings
  (Severity × Difficulty, short/long-term recommendations, effort estimate). Six dimensions in NFR
  priority order: security (OWASP Top 10:2025 RC, ASVS v5) → resilience → cost efficiency →
  architecture & coupling → documentation freshness → monitoring gaps. Never modifies the audited
  project — the only write is the report. Falls back to ripgrep heuristics when scanners
  (`gitleaks`, `trivy`, `semgrep`, `osv-scanner`, `infracost`, `checkov`) are absent. Five
  on-demand reference files (security/resilience/cost/docs+monitoring checklists, report
  template). `disable-model-invocation: true` — fires only on explicit `/code-audit` invocation.
  Optional flag: `--scope=security|resilience|cost|architecture|docs|monitoring|all`.

## [0.4.0] - 2026-04-17

### Added

- `deep-research` skill — multi-agent orchestrator producing long-form narrative research reports
  (3000-8000 words) grounded in the local codebase and the current web. 7-phase workflow:
  planner → codebase reconnaissance → user-approved outline → parallel web workers (3-5, hard cap
  20) → single-agent synthesis → citation attribution → validation. Output written to
  `./research/{date}-{slug}-report.md` on disk, not chat.
- Five plugin subagents backing the skill: `research-planner`, `code-reconnaissance`,
  `research-worker`, `research-synthesizer`, `citation-agent`.
- Validation scripts: `validate_report.py` (structure, word count, anti-plan-mode discipline),
  `verify_citations.py` (URL liveness + DOI resolution).

## [0.3.0] - 2026-04-17

### Added

- `gh-fix-pr-review` skill — fix Claude Code Action review feedback on the current PR. Inspects
  both inline review threads and PR issue comments, picks the highest severity unaddressed issue
  by default, implements and verifies the fix, then commits and replies on the review.
  Flags: `--all`, `--skip-tests`, `--inline`, `--comments`, `--resolve`, `--dry-run`,
  `--pr <number>`.

## [0.2.0] - 2026-04-17

### Added

- `gh-create-issue` skill — produce well-defined GitHub issues (single, epic, or sub-issues)
  with label discovery and a TDD plan for implementation-ready issues.

## [0.1.0] - 2026-04-17

### Added

- Initial plugin scaffold. No skills yet.
