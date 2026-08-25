# Marksmanship Hunter — DungeonRoute refinements — 2026-08-25

## Summary
Latest SimulationCraft Midnight APL commits refine Marksmanship Hunter Mythic+ / DungeonRoute behavior after several rapid same-day iterations.

### Final retained changes
- DungeonRoute no longer treats Explosive Shot as a blind on-cooldown recast while an Explosive Shot is already active. The final logic preserves recasts when no Explosive Shot is active or when Unstable Trigger explicitly permits the intended merged/recast state.
- Sentinel adds a DungeonRoute Aimed Shot branch with Sentinel's Mark + Bulletstorm before the normal Rapid Fire branch in multi-target contexts.
- Existing priority-target protection for Season 2 4pc Explosive Shot spreading remains important.

### Same-day correction that must NOT be retained
An intermediate commit attempted to front-load Volley before Explosive Shot in DungeonRoute. The immediately-following commit explicitly reverted this as producing impractical/cursed opener behavior. Do not preserve that intermediate opener rule.

## Practical interpretation
This is not simply a new static priority list. It reinforces a DungeonRoute state model:
1. DAMAGE_OBJECTIVE (priority target vs aggregate damage)
2. HERO_STATE (Sentinel / Dark Ranger)
3. EXPLOSIVE_SHOT_ACTIVE / UNSTABLE_TRIGGER
4. SENTINEL'S_MARK / BULLETSTORM
5. normal ST/AoE action priority

Current Icy Veins 12.1 guidance already supports the broader practical principle that M+ priority damage should override small aggregate gains from spreading Explosive Shot. Exact 2026-08-24/25 DungeonRoute gates remain PROVISIONAL until more live high-end log sequences are inspected.

## Evidence
- SimC 91a1ce9: MM APL Updates (DRoute Improvements)
- SimC 599455c: Dark Ranger MM APL Updates — applies DRoute optimizations from Sentinel
- SimC f8fafc1: MM APL DRoute Updates — explicitly fixes impractical opener implications of the preceding commit
- Icy Veins 12.1 rotation changelog (Aug 16): removed spreading Explosive Shot across two targets when a priority target is active because the aggregate gain did not improve M+ key speed due to priority-damage loss

## Status
PROVISIONAL_HIGH_CONFIDENCE for the APL state-machine direction; LIVE_VERIFY for exact M+ thresholds and adoption frequency in top logs.
