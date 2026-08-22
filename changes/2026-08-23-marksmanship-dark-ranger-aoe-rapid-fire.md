# 2026-08-23 — Marksmanship Dark Ranger AoE Rapid Fire clipping removal

## Summary
SimulationCraft commit `b883647b707e1bdf7300bae5b97a11e6d47a2489` (2026-08-22 UTC) removes the `interrupt_if` final-boundary Rapid Fire clipping clause specifically from the Dark Ranger AoE APL.

This narrows the previous 12.1 rule. Rapid Fire clipping is no longer a generic AoE behavior:

- Single-target, both hero trees: complete Rapid Fire.
- Dark Ranger AoE: complete Rapid Fire by default.
- Sentinel AoE: the existing Unload + Precise Shots + final-boundary clipping branch remains available.

## Evidence conflict
Icy Veins added generic AoE clipping guidance on 2026-08-13, but that guide update predates the 2026-08-22 hero-specific Dark Ranger APL revision. Current Wowhead 12.1 priorities list Rapid Fire with Trick Shots in AoE but do not provide stronger evidence that Dark Ranger should continue clipping. The newer SimC APL is maintained by Azortharion, who is also the current Wowhead/Trueshot Lodge theorycraft author, so the newer hero-specific revision carries high theoretical weight pending a guide refresh and live-log validation.

## Automation impact
Evaluate `HERO_STATE` before Rapid Fire channel policy:

```text
if SINGLE_TARGET:
    MUST_COMPLETE
elif DARK_RANGER_AOE:
    MUST_COMPLETE
elif SENTINEL_AOE and Unload and PreciseShots and ticks_remain < 2 and GCD_ready:
    CAN_CLIP
```

Do not reuse a single `AOE => clip` switch.

## Classification
- Dark Ranger AoE no-clip: `PROVISIONAL_HIGH_CONFIDENCE`
- Sentinel AoE final-boundary clip: retained `PROVISIONAL_HIGH_CONFIDENCE`
- Live-log confirmation: `LIVE_VERIFY`
