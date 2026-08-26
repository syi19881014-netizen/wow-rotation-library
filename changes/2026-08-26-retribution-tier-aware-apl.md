# Retribution Paladin — 2026-08-25 tier-aware APL update

## Evidence

- SimulationCraft PR #11802 / merge commit `c4acd568b0f15dc66aed16fef060a8b66a7da6eb`, followed immediately by generated APL update commit `1e025d2c2d9936e295334871d7b5854d6f0ccb90`.
- PR description: updated for tier changes and added an optional alternative cooldown sequence variable.
- Method 12.1 guide (updated 2026-08-12) still recommends the practical Templar Execution Sentence opener: Blade of Justice -> Avenging Wrath -> Execution Sentence -> Wake of Ashes -> Hammer of Light.
- Wowhead 12.1 guide (updated 2026-08-17) continues to frame the rotation around Holy Power/proc waste prevention and the conventional cooldown ordering; it has not yet documented the new optional Wake-first branch.
- Blizzard August 25 hotfixes do not prescribe a new Retribution PvE cooldown sequence, so this remains a theorycraft/APL development rather than an official rotation instruction.

## Material changes

1. New `pre_es_wake` strategy variable, default `0`.
   - `0`: preserves the existing practical Execution Sentence -> Wake of Ashes relationship.
   - `1`: allows Wake of Ashes before Execution Sentence. With Crusade talented, Execution Sentence then requires at least 7 Avenging Wrath/Crusade stacks.
   - This is an optional branch and must not overwrite the standard opener without live evidence.

2. Divine Storm admission simplified for the new tier environment.
   - Current theoretical gate is broadly `(2+ targets OR Divine Arbiter Verdict) AND not Empyrean Legacy AND not Divine Arbiter Divine Storm`.
   - Several former Empyrean Power / Seal of the Templar / Dawnlight sub-gates were removed from the APL expression.

3. Hammer of Light no longer globally blocks ordinary spenders.
   - Hammer of Light is still evaluated first.
   - If its own free-proc/hold condition says not to cast yet, Divine Storm or Templar's Verdict may now proceed instead of being vetoed solely because Hammer of Light is ready.

4. Walk Into Light adds explicit Hammer of Wrath 2-charge protection before the generic finisher call.

## Practical interpretation

- SimC theoretical benchmark should support both cooldown branches.
- Public high-end guides still favor Execution Sentence -> Wake, so that remains the practical production default.
- Wake-first / Crusade-7-stack -> Execution Sentence is `PROVISIONAL / LIVE_VERIFY` until top Season 2 raid/M+ sequences show repeatable practical adoption and no loss of total cooldown uses.
- The previously tracked Hammer of Light -> 9 Crusade stacks live-bug behavior stays isolated; the new alternate sequence must not be coupled to that temporary bug.

## Automation implications

Required state order:

`COOLDOWN_STRATEGY -> CRUSADE_STACKS -> EXECUTION_SENTENCE_ADMISSION -> WAKE_OF_ASHES_ADMISSION -> FINISHER_STATE -> PROC_OVERFLOW`

Runtime safeguards:

- expose `pre_es_wake_mode` explicitly rather than inferring it from target count;
- live-read Crusade stacks for the alternative branch;
- do not globally block spenders merely because Hammer of Light is ready;
- track Hammer of Wrath charges when Walk Into Light is selected;
- preserve fight-end / target-TTD overrides so alignment does not cost an entire cooldown use.

## Status

- APL structural update: `HIGH_CONFIDENCE_THEORY`
- Standard ES -> Wake practical opener: `STABLE_PRACTICAL`
- Wake -> build Crusade to >=7 -> ES alternative: `PROVISIONAL / LIVE_VERIFY`
- Exact Season 2 top-log adoption: pending.
