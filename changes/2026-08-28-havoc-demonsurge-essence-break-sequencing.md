# Havoc Demon Hunter — 2026-08-28 Demonsurge / Essence Break sequencing update

## Material change
SimulationCraft commit `e6671d8778e65ece4f6d9027875db32785763a5c` (`[Havoc] update APL per Shadarek`, 2026-08-27 UTC) changes the Season 2 Havoc burst ordering in a production-relevant way.

The new APL inserts Essence Break before pending Demonsurge Death Sweep / Annihilation consumption. Death Sweep remains admitted during Essence Break or when its Demonsurge variant is available; Annihilation is explicitly admitted both during Essence Break and when its Demonsurge variant is available. Ordinary A Fire Inside + Burning Wound Immolation Aura charge-pressure is moved below this burst package.

## Practical cross-check
Current Wowhead 12.1 guidance by Shadarek says Essence Break is generally cast on cooldown with the Season 2 4pc, except for a short hold when Eye Beam will be available within roughly four seconds. Current Method 12.1 Aldrachi Reaver single-target opener explicitly sequences `Eye Beam -> Essence Break -> Death Sweep -> Annihilation -> Death Sweep`.

This means the specific rule `Essence Break before consuming a legal Demonsurge spender` now has both SimC and current practical-guide support. Broader Season 2 simplifications remain provisional.

## Automation consequence
Recommended admission order:

1. Detect pending Demonsurge Death Sweep / Annihilation.
2. Evaluate whether Essence Break is legally available and valuable now.
3. If yes, cast Essence Break before consuming the Demonsurge spender.
4. Immediately re-evaluate Death Sweep / Annihilation inside the Essence Break window.
5. Only after this burst package, process ordinary Immolation Aura charge-pressure.

Do not convert this into an indefinite Demonsurge hold. Target death, fight end, range/movement failure, forced downtime, or unavailable Essence Break remain valid overrides.

## Status
- SimC theoretical rule: HIGH confidence.
- Essence Break-before-Demonsurge-spender production candidate: HIGH confidence.
- Exact encounter/M+ overrides and broader deleted old-APl heuristics: LIVE_VERIFY.
