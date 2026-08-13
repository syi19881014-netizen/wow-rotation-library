# 魔兽循环 / WoW Rotation Library — Agent Instructions

This repository is the authoritative Source of Truth for the “魔兽循环” project.

## Repository role
- `wow-rotation-library` = canonical rotation specifications and state-machine baselines.
- `wow-rotation-research` = public research/collection framework and generic tooling.
- `wow-rotation-data` = data/analysis artifacts; intended to remain private when it contains WCL samples or internal intermediate data.
- Future runtime/platform code should live in a separate private repository such as `wow-rotation-runtime`.

## Mandatory startup behavior for Codex / Work / agents
Before changing rotation logic:
1. Read `README.md`.
2. Read `library-index.yaml`.
3. Read `common/architecture.yaml`.
4. Read the target specialization's `baseline.yaml`.
5. Read `CHANGELOG.md` for recent superseded/live-verify context.
6. Do not reconstruct rotation rules from memory when repository content exists.

## Evidence model
SimulationCraft APL is a theoretical baseline, never the only source. For current WoW 12.1 / Midnight work, cross-check where relevant against:
- SimulationCraft APL and commits
- Warcraft Logs high-end casts/sequence evidence
- high-ranked raid/M+ gameplay
- class Discord/theorycrafter discussion
- Blizzard class/talent/set/trinket/mechanic changes
- current Wowhead/Icy Veins/Method/Maxroll and high-end creator material

## Status layers
- `STABLE`: sufficiently cross-validated; safe for production baseline.
- `PROVISIONAL`: high confidence but awaiting broader live validation.
- `LIVE_VERIFY`: logic is established/plausible, but exact numeric thresholds remain configurable.
- `FUTURE`: future set/season/build branch; must not pollute current launch baseline.
- `RESEARCH_CANDIDATE`: insufficient evidence for production.
- `SUPERSEDED`: historical rule retained for traceability; never use as current baseline.

## Update rules
- Material rotation changes should be committed in the same work session after evidence grading.
- Do not overwrite conflicting evidence silently; preserve scenario-specific branches.
- When new evidence replaces an old baseline, retain the old rule as `SUPERSEDED` with rationale.
- Keep exact seconds/stacks/resources/target-count thresholds configurable while marked `LIVE_VERIFY`.
- Keep Hero Talent, set bonus, build type, encounter context, and pre-pull logic as explicit isolated branches.
- Update `CHANGELOG.md` whenever a production-relevant rule changes.

## Automation architecture
Action priority is not enough. Evaluate in this order:
1. sensing/state inputs
2. cast/action admission
3. state-machine branch
4. action priority
5. execution policy

`CAST_POLICY` values:
- `MUST_COMPLETE`
- `CAN_CLIP_IF(condition)`
- `FREE_TO_INTERRUPT`

Pre-pull preparation must remain separate from combat rotation.

## Current project priority
For all-web update monitoring, inspect in this order:
1. Rogue — Assassination / Outlaw / Subtlety
2. Mage — Arcane / Fire / Frost
3. Death Knight — Blood / Frost / Unholy
4. Only if those have no material changes, continue to other classes/specs.

Only notify/commit changes that materially affect opener, rotation, burst, ST/AoE, resources, buff/debuff upkeep, target-count branches, talent/set/trinket interactions, or encounter strategy. Ignore formatting/comments/no-impact APL cleanup.

## Implementation goal
The end state is platform-independent rotation specs first, then adapters for runtime platforms. Quality target is:
1. statistical equivalence to SimC under comparable Patchwerk conditions,
2. expert/high-end practical strategy enhancements,
3. machine-execution enhancements where APIs permit.

Never sacrifice correctness merely to make a first version run.
