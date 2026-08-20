# 2026-08-21 — Marksmanship Aspect of the Hydra Rapid Fire tick correction

## Summary
SimulationCraft commit `48bf7e85fa6832a0c276bb26c0f00a67bdd817ae` (`[Hunter] Hydra Rapid Fire tick bugfix`, 2026-08-20 UTC) removes the prior bugs-mode rule that made Aspect of the Hydra's Rapid Fire secondary event trigger only on every other Rapid Fire channel tick. The current live model now triggers the Hydra secondary event on every Rapid Fire tick.

## Rotation impact
- This materially raises modeled Rapid Fire cleave value in Aspect of the Hydra builds, especially sustained 2-target / low-target aggregate-damage scenarios.
- It does **not** by itself justify a new GCD priority list. Existing Rapid Fire, Precise Shots, Trueshot/Explosive Shot alignment, and TOTAL_DAMAGE vs PRIORITY_TARGET routing stay in force.
- Any runtime damage forecast or target router using the superseded alternating-tick assumption must be corrected.
- M+ priority-target logic remains protected: higher Hydra cleave value is not permission to swap Rapid Fire away from an important priority target.
- Unload final-boundary clipping remains a separate `LIVE_VERIFY` execution optimization; verify that the final primary tick, Hydra secondary tick, and Unload event are all preserved before production promotion.

## Cross-source context
Current Wowhead Midnight Marksmanship material describes Aspect of the Hydra as a powerful 2-target cleave talent and says Hydra-based AoE follows the normal single-target priority naturally. Public Blizzard 12.1 notes describe Hydra's cleave coverage but do not document an every-other-tick Rapid Fire restriction. These sources are directionally consistent with removing the old half-frequency bug model, but current live WCL should still verify exact secondary-event counts.

## Status
- SimC mechanic correction: `HIGH`
- Practical priority rewrite: `NO`
- Runtime damage/target model update: `YES`
- Live log validation: `PENDING`
