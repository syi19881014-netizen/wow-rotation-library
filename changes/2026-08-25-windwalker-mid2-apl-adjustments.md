# 2026-08-25 — Windwalker Monk MID2 APL adjustments

## Summary
SimulationCraft commit `e4e92e9` (`[Monk] WW MID2 Profiles and small APL adjustments (#11788)`, 2026-08-24 UTC) enables the MID2 Windwalker raid profiles and materially refines Celestial Conduit / Zenith / Zenith Stomp admission. This is not just profile housekeeping.

## Material execution changes
- Celestial Conduit no longer relies on the older pair of pre-Conduit Blackout Kick / Tiger Palm fillers used solely while Zenith remained high. The current APL is more willing to release Conduit directly once the Heart of the Jade Serpent gates are satisfied.
- Celestial Conduit gains a multi-target/Bloodlust-or-DungeonSlice release branch: `active_enemies > 1 + Drinking Horn Cover rank` can admit Conduit under the corresponding context. Treat the exact target threshold as PROVISIONAL rather than a universal live rule.
- Zenith's Bloodlust/potion branch now also allows `Rushing Wind Kick` active to satisfy the release side of the condition.
- Zenith Stomp gains `cooldown.celestial_conduit.remains > 50` as an additional charge/resource-use admission in both ST and multi-target paths.
- In ST, the low-Chi Stomp branch no longer lets Combo Breaker block the cast when Zenith has <5s remaining.
- In multi-target, Zenith Stomp adds a Whirling Dragon Punch gate: normally avoid Stomp when WDP is ready unless Chi is very low (`chi < 2` in the benchmark APL).

## Cross-validation
Method's 12.1 Windwalker guide (updated 2026-08-18) agrees on the central practical structure: Celestial Conduit should generally wait until Heart of the Jade Serpent from WDP/Strike of the Windlord has expired, while Zenith Stomp is used for low Chi or late Zenith. Method also confirms the Season 2 4pc rule of using Spinning Crane Kick after Fists of Fury on 2+ targets. Peak of Serenity's 12.1 guide similarly treats Conduit as the higher-complexity/high-ceiling hero tree and recommends Shado-Pan for players who cannot execute Conduit reliably.

## Production recommendation
- Use the new SimC logic for the theoretical benchmark.
- Preserve `Heart -> Conduit` ordering as the practical stable concept.
- Keep the exact Conduit AoE target threshold, `>50s` Celestial Conduit cooldown Stomp threshold, and WDP/Chi gate configurable until Season 2 high-end logs validate them.
- For ordinary players, the stable simplification remains: do not overlap Conduit into an active Heart window; use Zenith Stomp when Chi is low / Zenith is expiring; consume S2 4pc with SCK after FoF on 2+ targets.

## Automation impact
Recommended state order:
`HERO_STATE -> SET_STATE -> HEART_STATE -> ZENITH_STATE -> TARGET_CONTEXT -> CELESTIAL_CONDUIT_ADMISSION -> ZENITH_STOMP_ADMISSION -> CORE_PRIORITY`.

Do not flatten Conduit and Shado-Pan, and do not hard-code SimC's exact target/cooldown thresholds while they remain live-validation parameters.
