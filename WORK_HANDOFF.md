# Work / Codex Handoff

This file exists so ChatGPT Work, normal Chat, and Codex can resume the “魔兽循环” project from the same durable context instead of relying on conversation history.

## Source of Truth
Canonical repository: `syi19881014-netizen/wow-rotation-library` (private).

Always prefer repository state over remembered chat state. If chat memory conflicts with this repository, inspect the latest commit and `CHANGELOG.md` before proceeding.

## Repository map
- `README.md` — library purpose and status semantics
- `library-index.yaml` — coverage index
- `common/architecture.yaml` — common automation/state-machine architecture
- `rogue/*/baseline.yaml` — Rogue baselines
- `mage/*/baseline.yaml` — Mage baselines
- `death-knight/*/baseline.yaml` — DK baselines
- `monk/windwalker/baseline.yaml` — Windwalker baseline
- `druid/guardian/baseline.yaml` — Guardian baseline
- `CHANGELOG.md` — material updates and superseded logic
- `AGENTS.md` — agent operating rules

## Current durable decisions
- New material rotation findings may be automatically committed after evidence grading; no separate user confirmation is required.
- Preserve `STABLE / PROVISIONAL / LIVE_VERIFY / FUTURE / RESEARCH_CANDIDATE / SUPERSEDED` separation.
- Do not let future Season 2 set logic contaminate launch-week/no-new-set baselines.
- Source conflicts are retained with applicability notes, not flattened into one unsupported answer.
- For automation, cast/action admission is evaluated before raw skill priority.

## Monitoring order
1. Rogue
2. Mage
3. Death Knight
4. Other classes only when the first three have no material update worth notifying.

## Cross-repository roles
- `wow-rotation-research` (public): research/collection framework, workflows, generic tooling.
- `wow-rotation-data`: WCL/data and analysis artifacts; should be private if it contains internal or granular datasets.
- `wow-rotation-library` (private): canonical rotation knowledge and machine-oriented specifications.
- Future platform/runtime/adapters/auth code: create a separate private repo, recommended `wow-rotation-runtime`.

## Resume protocol
When starting a new Work or Codex task for this project, use this instruction:

> Read `AGENTS.md`, `WORK_HANDOFF.md`, `README.md`, `library-index.yaml`, `common/architecture.yaml`, the relevant specialization `baseline.yaml`, and `CHANGELOG.md` before making changes. Treat the repository as the Source of Truth and preserve all status-layer semantics.

Last synchronized: 2026-08-13.
