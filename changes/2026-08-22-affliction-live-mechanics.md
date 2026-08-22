# 2026-08-22 — Affliction live mechanic/state corrections

SimulationCraft commit `22b442e063a3127b7452092e76bb90416bed5d6d` applies current live/hotfix behavior for Affliction and refines Season 2 4pc Unstable Affliction, Hellcaller Blackened Soul, Death's Embrace and Shard Instability semantics.

## Production impact

Core player-facing GCD priority is **not rewritten** by this record. Current Wowhead and Method 12.1 guides still support the normal DoT-maintenance, shard-management, UA/Seed spender and Dark Harvest framework.

The material automation changes are state/origin handling:

- Wither counts as an Affliction DoT for effects such as Withering Bolt.
- Season 2 4pc UA applied by Seed of Corruption increments Hellcaller Blackened Soul / Wither stacks on impact.
- Fatal Echoes UA does not increment Blackened Soul / Wither stacks.
- Seed-applied tier UA does not consume Succulent Soul and does not trigger Cull the Weak Dark Harvest CDR.
- Seed of Corruption plus its attached tier-UA consume at most one Shard Instability stack for the sequence.
- Malefic Grasp currently benefits from Death's Embrace and Withering Bolt.
- SimC current live-bug model indicates UA does not benefit from Death's Embrace while a seed-applied 4pc UA stack is present; retain this as `LIVE_VERIFY` rather than a permanent class rule.

## Runtime consequence

Do not represent every Unstable Affliction application with a single generic callback. Track at minimum `MANUAL_UA`, `FATAL_ECHOES_UA`, and `SEED_4PC_UA`, then resolve Blackened Soul, Succulent Soul, Cull the Weak, Shard Instability and execute modifiers according to application origin.

## Evidence grade

- SimC live mechanic implementation: high confidence for benchmark/state modeling.
- Current public practical guides: no priority rewrite yet.
- Season 2 WCL/theorycrafter validation: still required before changing talent/build or GCD-priority baselines.
