# Changelog

## 2026-08-14 — Arms Slayer Executioner's Precision execute update

### Provisional / practical split preserved
- SimulationCraft commit `f61263a` (2026-08-13) changed the Slayer Execute APL so Mortal Strike is used whenever Executioner's Precision reaches 2 stacks; the previous extra gate `(Rend remains <2s OR Martial Prowess stacks=3)` was removed.
- Current Wowhead Midnight Arms practical priority agrees with 2-stack Executioner's Precision -> Mortal Strike for Slayer Execute, but explicitly limits this behavior to Colossus Smash windows in Mythic+ builds.
- Current Method 12.0.7 Arms Execute rotation is even more conservative and requires both 2 Executioner's Precision stacks and Colossus Smash on the target.
- Added `warrior/arms/baseline.yaml` as `PROVISIONAL`: raid/ST branch admits Mortal Strike immediately at 2 EP stacks after higher cooldown priorities; M+ branch retains the Colossus Smash gate pending live-log/theorycrafter validation.
- The former generic `EP=2 AND (Rend<2s OR Martial Prowess=3)` rule is retained as `SUPERSEDED` for traceability.

## 2026-08-14 — Assassination Season 2 APL divergence

### Provisional / conflict preserved
- SimulationCraft commit `bf2d661` (2026-08-13) merged the Season 2 Assassination APL/profiles with numerous APL fixes.
- The Season 2 SimC branch removes the old Deathstalker M+ single-target Fan of Knives / ~3 Unshakeable Drive Mutilate exception and returns low-target generation to Mutilate/Ambush, while Fan of Knives is target-count gated with Blindside/Clear the Witnesses state.
- This directly conflicts with the currently published Wowhead 12.1 PTR practical guide, which still recommends Fan of Knives in single target for the M+ build and Mutilate at three Unshakeable Drive stacks.
- Added `rogue/assassination/season2-apl-2026-08-13.yaml`; do not overwrite the launch/live-guide branch until Season 2 live WCL or updated theorycrafter evidence resolves the conflict.
- SimC also adds Scent of Blood AoE cooldown-admission gating, tighter Envenom late-refresh/anti-overcap conditions, and a dedicated Font of Venomous Rage item branch.

## 2026-08-13 — Initial project-library bootstrap

Imported all rotation-library decisions that had been explicitly accepted or marked for inclusion in the “魔兽循环” project up to this date.

### Stable / production-facing
- Outlaw Rogue MID2 CD arbitration: Supercharger / BtE / Killing Spree / Preparation / Adrenaline Rush.
- Subtlety Rogue 12.1 Hero Talent split, Dance admission, Goremaw and Ancient Arts/Apex state handling.
- Fire Mage Pyroclasm protected-cast architecture, corrected to allow valid off-GCD Fire Blast weaving.
- Frost Mage Frostfire/Spellslinger split and dynamic spell-admission conditions.
- Frost DK Pillar/Frostwyrm alignment, Killing Machine consumption and target-count branches.
- Windwalker Monk MID2 resource/proc/burst state architecture.
- Guardian Druid Rage/Apex arbitration and survival reserve layer.
- Common Cast Admission / CAST_POLICY architecture.

### Provisional / live verification
- Assassination Rogue Implacable Envenom late-refresh threshold.
- Assassination Deathstalker M+ FoK default builder / ~3 Unshakeable Drive Mutilate threshold.
- Fire Mage no-Heating-Up Pyroclasm Fire Blast weave timing and possible double-weave.
- Arcane Mage competing opener variants.
- Unholy DK Epidemic ordering.

### Future set branches
- Outlaw Rogue S2 Fang Strike early Dispatch.
- Blood DK S2 Blood Debt / Marrowrend.
- Windwalker Monk S2 4pc FoF -> SCK; pure-ST SCK remains LIVE_VERIFY.

### Superseded
- Assassination: intentionally dropping Envenom as a fixed Implacable optimization target.
- Fire: blanket prohibition on Fire Blast during Pyroclasm Flamestrike hardcast.
