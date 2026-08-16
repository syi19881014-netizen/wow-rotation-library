# Changelog

## 2026-08-16 — Balance Druid AoE / DungeonRoute APL correction

### Provisional Season 2 AoE execution branch added
- SimulationCraft commit `f172b7e` (2026-08-16 UTC), mirrored directly from Dreamgrove commit `955fc4a`, materially changes Balance AoE execution rather than formatting only.
- DungeonRoute Eclipse admission now waits until the current Eclipse has <2s remaining before the normal timing branch can trigger, reducing premature Eclipse transitions during active pack windows.
- Generic major-cooldown admission now requires `target.time_to_die > 20s`; this affects CA/Incarnation and aligned minor-CD windows and is intended to stop full cooldown packages from being dumped into packs/targets that will die too quickly.
- Moonfire multi-dotting is now explicit on 2+ targets: refreshable targets are selected only when effective remaining life exceeds ~6s and active Moonfires remain below 10; single-target Elune's Chosen Moonfire upkeep is separated with a longer line cooldown.
- Starfall now retains the `target_ttd > 5s` gate even when admitted by Starweaver's Warp / Touch the Cosmos, and the opener can choose Starfall rather than forcing Starsurge on 2+ targets when the AoE spender conditions are met.
- Public Wowhead and Method Balance guides are still 12.0.7-era, so these exact 12.1 thresholds remain `PROVISIONAL / LIVE_VERIFY`; Dreamgrove + SimC agreement is strong theorycraft evidence but Season 2 live WCL should still validate practical pack/TTD behavior.
- Added `druid/balance/baseline.yaml` and indexed it. Runtime TTD thresholds must stay configurable and encounter-overridable rather than treated as perfect truth.

## 2026-08-16 — Subtlety Deepening Shadows dynamic duration behavior

### Runtime state tracking changed; core priority remains unchanged
- SimulationCraft commit `85cb849` (2026-08-15 UTC) updates Subtlety's Deepening Shadows behavior from a cast-time-only assumption: current testing indicates that if haste increases while Shadow Dance is already active, the existing Dance can be extended when the newly calculated hasted duration is greater than its original trigger duration.
- The modeled behavior does not shorten an already active Shadow Dance when haste falls; it only extends on a qualifying higher-duration recalculation.
- This is a material automation/state-model change even though the APL priority itself was not rewritten: runtime code must not freeze Shadow Dance expiry using only the haste value present at cast time. Prefer live buff-remains reads each tick, or refresh expected expiry when haste changes.
- Current Wowhead practical guidance still treats Deepening Shadows as highly timing-sensitive, including one-second update behavior and macro/timing tricks to avoid losing the final Dance GCD. The new SimC implementation suggests later haste gains can extend an active Dance, but it does not yet prove those practical methods are obsolete in every trinket/haste case.
- Updated `rogue/subtlety/baseline.yaml` as `PROVISIONAL / LIVE_VERIFY` for the dynamic-duration semantics; no deliberate hold/proc-fishing rule is added without live-log/theorycrafter evidence.

## 2026-08-16 — Marksmanship Rapid Fire clipping promoted into SimC APL

### Theoretical branch strengthened; practical production still LIVE_VERIFY
- SimulationCraft commit `6cad471` (2026-08-15 UTC) changes the actual Marksmanship APL, not just the bug model: Rapid Fire now uses `interrupt_if=ticks_remain<2&buff.precise_shots.up&!gcd.remains` with immediate/global interrupt flags in Dark Ranger and Sentinel single-target and AoE branches.
- This materially strengthens the previous Unload/Precise Shots clipping finding from a modeled mechanic into an explicit SimC theoretical rotation rule.
- Current public Wowhead and Method Marksmanship rotation guides still describe the normal full-channel Rapid Fire -> Precise Shots spender flow and do not document deliberate final-boundary clipping, so the practical production default remains full channel pending Season 2 logs/theorycrafter validation.
- Updated `hunter/marksmanship/baseline.yaml`: the machine branch now mirrors SimC's exact admission shape (`ticks_remain < 2`, Precise Shots active, GCD free) while preserving safeguards against earlier clips or generic channel cancellation.

## 2026-08-16 — Outlaw 12.1 WCL/API validation and runtime corrections

### Current spell identities and Coup admission validated
- Collected eight high-ranked 12.1 PTR Mythic+ Outlaw reports across eight dungeons through the official Warcraft Logs API v2. Browser scraping was not used; whole-dungeon data remains `LIVE_VERIFY` because travel, AoE and encounter downtime make it unsuitable as a direct Patchwerk target.
- Confirmed current event spell IDs for Roll the Bones (`1214909`), Tricks of the Trade (`1224098`) and Coup de Grace (`441776`). Legacy Roll the Bones IDs remain runtime aliases only.
- Removed the unsupported player-buff gate from Coup de Grace admission. Both current SimC APL behavior and the WCL event streams use Coup as a talent/hero-available, cooldown-ready finisher without a matching prerequisite player buff.
- Added an isolated current MID1 two-piece branch: Blade Rush is admitted whenever ready and position-safe only when equipped-set detection or an explicit runtime setting confirms the bonus.
- Recorded a comparable 300-second single-target diagnostic: about `80.4k` with no raid buffs or combat potion but with food, flask, augmentation rune, both weapon oils and a substituted item-level-272 boot; removing those non-potion consumables reduces the result to about `74.1k`.
- Added a `LIVE_VERIFY` smart-burst branch from the same eight WCL runs. Their dungeon completion median is `29.17m`, and 352 adjacent Adrenaline Rush intervals have a `34.79s` median (`30.79–44.21s` interquartile range), showing little support for long generic holds.
- Cast-gap pull inference is explicitly treated as uncertain: at a 2.5s boundary, AR windows have a `35.51s` median and `22.78s` lower quartile, versus `14.95s` median and `23.28s` upper quartile without AR. The runtime small-pull TTD default is therefore raised from `15s` to a configurable `20s`, while bosses and 4+ targets remain immediate; adding a target within the same pack no longer resets the decision.

## 2026-08-15 — Frost DK Season 2 tuning redistribution

### Provisional tuning impact recorded; core rotation unchanged for now
- SimulationCraft commit `fcb291e` (2026-08-15 UTC, labelled "Overrides for next weeks tuning") adds live hotfix overrides for Death Knight tuning expected with the next weekly reset.
- Frost receives a net +9% baseline aura shift (`-12% -> -3%` in the SimC aura override), while the Season 2 Freezing Tempest package is cut in half: attack speed per stack `2% -> 1%`, and Icy Death Torrent bonus per stack `4% -> 2%`.
- This is a material redistribution away from the S2 tier/IDT package and toward ordinary rotational abilities, but there is no accompanying Frost APL rewrite yet. Therefore the production priority remains unchanged.
- Pre-tuning public 12.1 material describes the Frost S2 2pc as `2%` attack speed + `4%` IDT damage per Freezing Tempest stack and community/theorycraft feedback highlighted strong IDT/uptime and dual-wield sensitivity. The post-tuning split should reduce that dependence, but exact DW-vs-2H/build impacts remain `LIVE_VERIFY` until updated sims and Season 2 logs are available.
- Updated `death-knight/frost/baseline.yaml`: do not invent a new GCD/hold rule solely to chase Freezing Tempest; re-sim weapon/build gaps and movement-sensitive stack value after the tuning lands.
- The same SimC tuning commit also halves Blood Transfusion (`10% -> 5%`) and reduces Blood Visceral Strength (`10% -> 6%`). These are tracked as balance/build-pressure changes only; they do not yet justify rewriting Blood/Unholy production rotations.

## 2026-08-15 — Havoc Demon Hunter Midnight Season 2 APL rewrite

### Provisional Season 2 theoretical branch added
- SimulationCraft PR `#11740`, merged as `4b45fc2b` on 2026-08-14, wholesale rewrites the Havoc Midnight Season 2 APL: the old heavily conditional core is replaced by a compact priority list.
- Removed SimC helper trees include old Fury forecasting, `use_blade_dance` target-count gating, broad Inertia consumer timing variables, `eb_aligned`, Burning Wound retarget heuristics, and several fine-grained filler/cooldown checks.
- The new APL explicitly prioritizes pre-Metamorphosis Immolation Aura for Violent Transformation + A Fire Inside, Metamorphosis admission after pending Demonsurge checks, The Hunt gating around Eye Beam/Meta/Eternal Hunt/Reaver's Glaive state, Eye Beam as a direct high-priority action, Essence Break when Eye Beam remains >4s, and Death Sweep during Essence Break/Demonsurge windows.
- Current Wowhead and Method public Havoc guides are still 12.0.7-era. Wowhead continues to emphasize keeping Blade Dance/Immolation Aura/Eye Beam on cooldown and pairing Essence Break and Vengeful Retreat with Eye Beam; Method likewise retains the older Fel-Scarred/Inertia practical framework.
- Added `demon-hunter/havoc/baseline.yaml` as `PROVISIONAL`: use the new SimC branch for Season 2 theoretical benchmarking, but do not overwrite the practical production baseline until Season 2 live raid/M+ logs and updated theorycrafter guides validate the simplified ordering.

## 2026-08-15 — Marksmanship Hunter Unload / Precise Shots channel-end interaction

### Provisional machine-execution branch added
- SimulationCraft commit `94f7e089` (2026-08-14) models a live Unload timing quirk: the second Unload shot at Rapid Fire channel end consumes Precise Shots on a slight delay (10 ms in SimC bugs mode).
- The commit explicitly notes that this permits a Precise Shots spender to be queued/clipped at the Rapid Fire channel boundary so both the second Unload shot and the Arcane Shot/Multi-Shot spender can benefit before Precise Shots is consumed.
- Current Wowhead and Method Marksmanship practical guides still describe the normal flow as keeping Rapid Fire/Aimed Shot rolling and spending Precise Shots immediately after the generator; neither currently documents deliberate Unload channel clipping.
- Added `hunter/marksmanship/baseline.yaml` as `PROVISIONAL`: retain normal full-channel -> Precise Shots spender as the production/practical default, but expose a machine-execution `CAN_CLIP_IF(...)` branch at the final channel boundary.
- Exact timing, latency tolerance, and preservation of the final Rapid Fire tick are `LIVE_VERIFY`; remove the optimization if Blizzard fixes the delayed-consumption behavior.

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
