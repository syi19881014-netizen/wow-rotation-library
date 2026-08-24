# 2026-08-24 — Marksmanship Moonlight Chakram Trueshot timing

## Summary
SimulationCraft commit `9ad671eaff67a2344a3ec0646f0d7d4a2557cf52` corrects Moonlight Chakram's live bounce timing and changes the Sentinel single-target APL so the late Trueshot Chakram is admitted much earlier.

## Mechanic correction
- Single-target bounce interval: roughly 640 ms.
- Multi-target bounce interval: roughly 130 ms.
- Previous SimC estimate was 200 ms for all contexts.

Because the single-target sequence is substantially slower, the Sentinel ST APL changes the late Trueshot condition from `buff.moonlight_chakram.remains < gcd.max` to `< 5` seconds. The stated goal is to keep all Chakram bounces inside Trueshot.

## Practical conflict
Method 12.1 (updated 2026-08-17) and Wowhead 12.1 (updated 2026-08-15) still describe Moonlight Chakram as an end-of-Trueshot filler / cast when Trueshot is about to expire. Those guides predate this 2026-08-24 travel-time correction and do not distinguish the much slower ST bounce sequence from the faster AoE sequence.

## Automation impact
Moonlight Chakram must be modeled as future bounce events rather than instant-realized damage. In Sentinel ST, CAST_ADMISSION should consider Trueshot remains and leave enough time for the full bounce sequence; the current SimC theoretical benchmark uses a 5-second remaining threshold. Do not apply the same threshold mechanically to AoE because the bounce cadence is much faster there.

## Status
- Travel/bounce model: `PROVISIONAL_HIGH_CONFIDENCE`.
- SimC benchmark ST containment threshold (~5s): `PROVISIONAL_HIGH_CONFIDENCE`.
- Exact live production margin: `LIVE_VERIFY` pending current high-end logs / refreshed guides.

## Library update
Updated `hunter/marksmanship/baseline.yaml` with a dedicated `moonlight_chakram_trueshot_containment` branch and target-count-aware timing safeguards.
