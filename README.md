# WoW Rotation Library — 12.1 / Midnight

Machine-oriented rotation research library for the “魔兽循环” project.

## Status layers
- `STABLE`: sufficiently cross-validated; safe as current production baseline.
- `PROVISIONAL`: high-confidence live baseline, but still awaiting broader live validation.
- `LIVE_VERIFY`: logic is plausible/established, but exact thresholds/timings must remain configurable.
- `FUTURE`: future set/season/build branch; must not pollute the current Launch Week baseline.
- `RESEARCH_CANDIDATE`: evidence insufficient for production use.
- `SUPERSEDED`: retained for history; do not use as current baseline.

## Design principles
1. SimC APL is the theoretical baseline, never the only source.
2. Hero Talent, set bonus, build type and encounter context are explicit branches.
3. Cast admission is evaluated before raw action priority.
4. Pre-pull logic is separate from combat logic.
5. Live thresholds remain configurable until frozen by high-quality live evidence.
6. When a baseline is replaced, preserve the superseded rule and rationale.

## Current coverage
- Rogue: Assassination, Outlaw, Subtlety
- Mage: Fire, Frost, Arcane
- Death Knight: Blood, Frost, Unholy
- Monk: Windwalker
- Druid: Guardian
- Common automation architecture

Last synchronized from project memory: 2026-08-13.
