# 2026-08-27 — Windwalker Conduit admission/resource-state update

## Summary
SimulationCraft commit `a8bde88` (`[Monk] MID2 Profile update & APL changes`, 2026-08-26 UTC) materially changes Windwalker Conduit of the Celestials execution. This is not treated as a flat priority-list rewrite; it refines burst admission and resource setup.

## Material changes
- The main Conduit/Xuen line now requires `chi>2` or `chi>1 && energy<40`, reducing entries into the major burst package from a weak resource state.
- A dedicated low-Chi Zenith Stomp line (`chi<2` or Zenith nearly expired) is evaluated before Celestial Conduit in the Conduit action list.
- On single target without Harmonic Combo, Tiger Palm can be used during Zenith at very low Chi when no Heart window is active, specifically as resource setup rather than as generic filler delay.
- Most importantly, single-target Celestial Conduit is now present in the core ST list without requiring Zenith to be active. The primary gate is no active Heart of the Jade Serpent / Yu'lon Heart plus the WDP / Strike of the Windlord state.
- The broader old ST Zenith Stomp line is restricted to Flurry Strikes, keeping Conduit's Stomp usage in its dedicated setup branch.
- Zenith's Bloodlust/potion release line is slightly more permissive when Fists of Fury is ready and no Heart/Unity Within Heart is active.

## Cross-check
- Method 12.1 (updated 2026-08-18) already states that Conduit should be used when no Heart of the Jade Serpent window is active and warns against opening Conduit too early into Heart. It also describes Zenith Stomp as a low-Chi / Zenith-expiry tool.
- Wowhead's Windwalker rotation page was updated 2026-08-25 and continues to frame Windwalker as a resource-aware priority system rather than a fixed sequence.
- Therefore the Heart-state gate is strong practical evidence; the exact Xuen Chi/Energy threshold and the new no-Zenith-required ST Conduit admission remain `PROVISIONAL_HIGH_CONFIDENCE` pending Season 2 log validation.

## Automation impact
Recommended ordering:

`HERO_STATE -> SET_STATE -> HEART_STATE -> RESOURCE_STATE -> XUEN_ADMISSION -> ZENITH_STATE -> TARGET_CONTEXT -> CELESTIAL_CONDUIT_ADMISSION -> ZENITH_STOMP_ADMISSION -> CORE_PRIORITY`

Do not model Zenith as a universal prerequisite for Celestial Conduit. Keep the exact Chi/Energy and target-count thresholds configurable and preserve encounter/fight-end overrides.
