# Blood DK Echoing Fury / Reaper's Mark correction — 2026-08-20

## Summary
SimulationCraft commit `980e11204d5c8c388a08c428c76df54e3933cb9d` (2026-08-19 UTC) removes the Blood Death Knight bug model that granted an Echoing Fury Exterminate stack immediately when Reaper's Mark was cast.

## Current rule
- Casting Reaper's Mark must **not** create an Echoing Fury Exterminate stack for Blood.
- Dancing Rune Weapon remains the Blood Echoing Fury trigger for its extra Exterminate stack.
- The normal Exterminate effect produced by Reaper's Mark detonation remains separate and unchanged.
- Runtime prediction must not assume an extra pre-detonation Exterminate charge from the Reaper's Mark cast itself.

## Evidence
- SimC commit `980e112` removes code whose comment said the behavior had been tested on June 11, 2026 but "should only apply to Frost".
- Current Wowhead Blood overview explicitly identifies Reaper's Mark triggering Echoing Fury on Blood as a bug; the intended Blood talent description ties the extra stack to Dancing Rune Weapon.

## Rotation impact
This is a **mechanic/state-forecast correction**, not a broad priority rewrite. Reaper's Mark remains an on-cooldown offensive action unless encounter timing overrides it. The main impact is Exterminate timing, Marrowrend/Bone Shield forecasting, and resource planning between Reaper's Mark cast and detonation.

## Status
`STABLE` for the trigger semantics; no new hold or opener rule is introduced solely from this fix.
