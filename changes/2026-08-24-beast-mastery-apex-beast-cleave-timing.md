# Beast Mastery Hunter — Apex Beast Cleave timing update

Date: 2026-08-24
Status: PROVISIONAL_HIGH_CONFIDENCE

## Material change
SimulationCraft commit `5d55482c9819b276a3d59880ca5d9996abf7eefa` changes Beast Mastery AoE execution around Bestial Wrath + Wild Thrash. The APL now:

1. forces Wild Thrash immediately after Bestial Wrath when Beast Cleave is talented and Wild Thrash is available;
2. otherwise holds Wild Thrash when Bestial Wrath will become ready before the current Beast Cleave expires;
3. immediately restores Beast Cleave if the buff is absent.

The stated mechanism is that Bestial Wrath spawns an Apex pet whose Bestial Wrath event occurs about 1.5 seconds later, but that Apex pet does not inherit the Beast Cleave buff that existed before Bestial Wrath. Reapplying Beast Cleave with Wild Thrash after Bestial Wrath allows the Apex event to cleave.

## Practical cross-check
Method's 12.1 Beast Mastery guide, updated 2026-08-17, independently recommends aligning Wild Thrash around Bestial Wrath and using Wild Thrash as the first GCD after Bestial Wrath whenever possible for this same Apex-pet reason. It also recommends, where possible, placing a Wild Thrash about two globals before Bestial Wrath so subsequent Wild Thrashes remain inside the damage window.

Wowhead's current public AoE priority is simpler: Wild Thrash precedes Bestial Wrath while Beast Cleave maintenance remains central. This does not contradict the Apex timing rule, but does not expose its full state-machine nuance.

## Automation consequence
Do not implement Beast Mastery AoE as `Wild Thrash on cooldown`. Use an explicit Beast Cleave / Bestial Wrath admission state:

- Beast Cleave down -> Wild Thrash now.
- Beast Cleave safely covers Bestial Wrath and BW is imminent -> hold Wild Thrash briefly.
- Bestial Wrath was the previous GCD -> Wild Thrash immediately if available.

The current SimC optimization describes a hold of up to roughly 2 seconds. Keep that threshold configurable and overridable by pack TTD, priority-target needs, latency/GCD timing, and pet travel.

## Evidence grade
- SimC theoretical branch: HIGH.
- Method practical agreement: HIGH for the timing concept.
- Exact hold duration in live M+/raid: LIVE_VERIFY.
