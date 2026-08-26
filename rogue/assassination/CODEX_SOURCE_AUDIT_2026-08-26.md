# Assassination Rogue 12.1 — Public Source Audit for Codex

## Objective
Perform a source-level audit of the current 12.1/Midnight Assassination Rogue rotation knowledge against pinned public implementations. Do not assume historical source logic is current merely because the implementation is sophisticated.

## Mandatory local reads
Before analysis, read:
1. `AGENTS.md`
2. `README.md`
3. `library-index.yaml`
4. `common/architecture.yaml`
5. `rogue/assassination/baseline.yaml`
6. `rogue/assassination/season2-apl-2026-08-13.yaml`
7. `rogue/assassination/external-source-catalog-2026-08-26.yaml`
8. `CHANGELOG.md`

## External sources
Use the exact repositories, files, and pinned commits in `external-source-catalog-2026-08-26.yaml`.

Priority:
1. SimulationCraft MID2 = current theoretical baseline.
2. HeroRotation = historical high-quality Lua implementation reference.
3. Hekili = historical APL/runtime translation reference.
4. !WR = historical full-auto execution/control-flow reference.

Do not use Cyber_Deck as a complete Assassination rotation; it is cataloged as excluded.

## Analysis method
Build a semantic model rather than a textual diff. Map equivalent concepts even when names differ.

Audit at least these domains:
- sensing/state inputs
- energy regeneration, pooling, overcap prevention
- combo-point generation/spending and overcap prevention
- Garrote maintenance and multidot targeting
- Rupture maintenance and multidot targeting
- Crimson Tempest admission/refresh logic
- Envenom admission, pooling and buff maintenance
- Vanish/Subterfuge/stealth-window logic
- Deathmark admission and alignment
- Kingsbane admission and alignment
- Shiv admission and alignment
- Thistle Tea usage
- Deathstalker Mark/Darkest Night state handling
- Fatebound branch where applicable
- single-target vs 2-target vs 3+ target branches
- target_if / off-target selection
- target time-to-die and fight-remains logic
- execute/end-of-pull dump behavior
- cooldown delay vs target death / next-pull value
- defensive, interrupt and utility integration boundaries
- runtime execution architecture that is reusable on Workout without importing stale combat rules

## Required classifications
For every meaningful rule/pattern, classify it as one of:
- `CURRENT_SIMC_BASELINE`
- `CURRENT_PROJECT_ALREADY_HAS`
- `PORTABLE_IMPLEMENTATION_PATTERN`
- `POTENTIAL_12_1_ENHANCEMENT`
- `LIVE_VERIFY_REQUIRED`
- `STALE_VERSION_LOGIC`
- `INVALID_FOR_12_1`
- `PLATFORM_SPECIFIC_ONLY`

## Special focus
Pay particular attention to the project's known hard problem: Garrote/Rupture refresh coordination in multi-target combat.

Do not reduce this to a fixed "refresh both together" rule. Determine from current MID2 logic and historical implementation patterns:
- when refreshing only one DoT is justified;
- when waiting for a better paired-refresh window is justified;
- when waiting becomes a DPS loss because of cooldown/burst timing or target TTD;
- whether Crimson Tempest should ever be used primarily as a refresh tool and under which conditions;
- what state variables are required for an automatic rotation to make that decision robustly.

Also audit Deathstalker mark target selection and protection from wasting/losing the mark on dying targets.

## Deliverables
Create these files only; do not modify `baseline.yaml` or production rotation rules during the first pass:

1. `rogue/assassination/audits/source-audit-2026-08-26.md`
   - executive findings
   - source quality/staleness assessment
   - semantic mapping by subsystem
   - discrepancies against current project baseline
   - candidate improvements

2. `rogue/assassination/audits/source-audit-2026-08-26.yaml`
   - machine-readable findings
   - classification for every candidate rule
   - evidence source + pinned commit/file
   - confidence
   - required runtime inputs
   - validation requirement

3. `rogue/assassination/audits/workout-portability-2026-08-26.md`
   - implementation ideas portable to Workout
   - required API/state capabilities
   - patterns that must not be copied because they rely on normal addon APIs, stale mechanics, or recommendation-only semantics

## Constraints
- Do not treat SimC as the only source of truth, but current MID2 mechanics outrank stale implementation rules.
- Do not silently change numeric thresholds.
- Preserve build/hero-talent/target-count branches explicitly.
- Distinguish combat-rule improvements from runtime-engine improvements.
- Do not change production baseline in this first audit pass.
- Cite exact repository, commit SHA, file path, and relevant function/action-list name for each substantive finding.
- Prefer semantic summaries over copying large source blocks.

## Completion criterion
The audit is complete only when it can answer: "What does each historical implementation teach us that current MID2 APL alone does not, and which of those lessons are safe or promising for a 12.1 automatic Assassination rotation?"
