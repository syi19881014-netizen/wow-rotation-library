# Marksmanship Hunter — Explosive Shot admission correction — 2026-08-25

## Summary
SimulationCraft commit `cd25e2c6fdb7cb7df94ce70ad840e96d902b5a6c` re-simplified the Marksmanship APL and removed the DungeonRoute hard gate that blocked the normal Explosive Shot action whenever an Explosive Shot DoT was already active unless Unstable Trigger was up.

This supersedes the 2026-08-24 interpretation that `active_dot.explosive_shot > 0` should globally veto the base/current-target Explosive Shot action in DungeonRoute.

## Current interpretation
- The normal Explosive Shot line should not be hard-blocked solely because an Explosive Shot is already active.
- Season 2 4pc secondary-target cycling remains separately restricted in DungeonRoute priority-damage contexts.
- Same-target repeat-cast admission and secondary-target spreading are different decisions and must not share one global veto.
- Tactical Reload / Lock and Load conditions remain explicit.

## Practical cross-check
Current Icy Veins 12.1 documentation describes Unstable Trigger as allowing a second Explosive Shot within 3 seconds while incorporating remaining damage, while separately retaining the practical M+ principle of not spreading Explosive Shot to secondary targets when that costs priority-target damage.

## Automation impact
Do not implement:

`DungeonRoute AND ExplosiveShotActive AND NOT UnstableTrigger => block ExplosiveShot`

as a universal rule.

Instead evaluate:
1. damage objective;
2. target routing / 4pc cycling policy;
3. Tactical Reload / Lock and Load gate;
4. normal Explosive Shot admission.

Any live same-target tick-loss exception should be a build-specific `LIVE_VERIFY` branch rather than restoration of the superseded global gate.

## Evidence status
- SimC APL change: HIGH
- Practical interpretation: PROVISIONAL_HIGH_CONFIDENCE
- Exact same-target timing by build: LIVE_VERIFY
