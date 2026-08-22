# 2026-08-23 — Marksmanship Rapid Fire ST/AoE context split

## Material change

SimulationCraft commit `a011ae422a21b9269a062c23c019c2996e1daa21` (2026-08-22 UTC) revises the 12.1 Marksmanship APL in four material ways:

1. Deliberate final-boundary Rapid Fire clipping is removed from single-target Dark Ranger and Sentinel lists.
2. The same Unload + `ticks_remain < 2` + Precise Shots + GCD-ready clipping remains in AoE.
3. Sentinel single-target gains a Rapid Fire line ahead of ordinary Precise Shots spenders when Precise Shots is already up and both Unload and No Scope are talented; therefore production logic must not globally require spending Precise Shots before every Rapid Fire.
4. Sentinel AoE Aimed Shot nameplate routing removes the `max_prio_damage` target gate, allowing marked-target hunting even in DungeonRoute when the modeled gain is worth some immediate priority-target loss.

## Practical cross-check

Current Icy Veins 12.1 guidance independently matches the most important context split: Rapid Fire is listed on cooldown in single-target priorities, while its AoE priority explicitly instructs clipping the end of Rapid Fire with Multi-Shot. This is stronger practical support than the previous universal clipping assumption.

Wowhead's current 12.1 ability guide continues to describe Rapid Fire as a core on-cooldown ability and documents No Scope/Unload/Precise Shots interactions, but does not supply evidence for systematic single-target clipping.

## Project baseline

- ST Rapid Fire deliberate clip: `SUPERSEDED`; production default `MUST_COMPLETE`.
- AoE final-boundary clip with Unload/Precise Shots/GCD gate: `PROVISIONAL_HIGH_CONFIDENCE` and compatible with current practical guidance.
- Sentinel ST Rapid Fire into an already-active Precise Shots state with Unload + No Scope: `PROVISIONAL_HIGH_CONFIDENCE`.
- Sentinel AoE Aimed Shot marked-target hunting in DungeonRoute: `PROVISIONAL`; retain encounter must-kill override.

Updated `hunter/marksmanship/baseline.yaml` in commit `1cae67ec80feef01654eb8f6ec809055a2be45bf`.
