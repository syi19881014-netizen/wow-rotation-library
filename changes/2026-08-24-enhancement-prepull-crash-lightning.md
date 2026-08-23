# Enhancement Shaman — Pre-pull Crash Lightning opener update

Date: 2026-08-24

## Material change
SimulationCraft commit `e6f5b00266c5591c26ffb72ffc1fc82d6935f9e1` adds an explicit precombat Crash Lightning execution mode. The modeled pre-pull action triggers the Crash Lightning buff without dealing damage, creating puddles, consuming Maelstrom on a hit, or initiating combat.

Current Wowhead 12.1 Enhancement opener guidance independently documents a practical version of the same tactic: pre-cast Crash Lightning roughly one second before pull in Stormbringer single-target and AoE openers, and in the published Totemic single-target opener. The published Totemic AoE sample does not require it, so this must remain a build/route/encounter branch rather than a universal opener.

## Automation impact
Add a dedicated `PREPULL_PREPARATION` state before combat rotation. When a valid pull timer exists and the runtime can safely execute the harmless setup action, submit Crash Lightning once roughly 1.0s before pull, then re-read the real Crash Lightning buff and cooldown at combat start. Do not let the normal combat Crash Lightning handler double-submit the setup cast.

If the pull timer is unreliable, the character is already in combat, a chain-pull is occurring, or platform semantics cannot distinguish the harmless precombat setup from a real harmful cast, skip this optimization.

## Evidence grading
- SimC implementation / mechanism: HIGH
- Stormbringer practical opener: HIGH (current Wowhead 12.1 sequence)
- Exact 1.0s timing: PROVISIONAL / latency configurable
- Universal use in all Totemic/AoE routes: NOT SUPPORTED

## Library action
Updated `shaman/enhancement/baseline.yaml` with `precombat_crash_lightning` and PREPULL runtime safeguards. Core in-combat priority remains unchanged.