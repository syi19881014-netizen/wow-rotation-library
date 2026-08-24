# Beast Mastery Dark Ranger — Wild Thrash / Bestial Wrath timing update

Date: 2026-08-25
Status: PROVISIONAL_HIGH_CONFIDENCE

## Material change

SimulationCraft commit `25faf4d747e9c4c655d86befb5f54162c43620d6` applies the same Wild Thrash holding logic already used by Pack Leader to Dark Ranger AoE.

The Dark Ranger cleave APL changed from unconditional `wild_thrash` to a stateful condition: use Wild Thrash when Beast Cleave is absent, when the previous GCD was Bestial Wrath, or when Bestial Wrath will not become ready before Beast Cleave expires. Bestial Wrath remains gated on Beast Cleave being active when Beast Cleave is talented.

## Practical cross-check

Method 12.1 guidance (updated 2026-08-17) already recommends the same concept for Dark Ranger AoE: when possible, use Wild Thrash around two globals before Bestial Wrath and make Wild Thrash the first global after Bestial Wrath. This is intended to ensure the newly spawned Apex pet gains Beast Cleave before its delayed Bestial Wrath damage event (~1.5s after the main pets).

Wowhead 12.1 guidance remains a simpler human-facing priority and does not fully encode the hold window; it is therefore treated as the stable approximation, not a contradiction.

## Automation impact

The runtime must no longer restrict `HOLD_WILD_THRASH_FOR_BW` and `FORCE_POST_BW_WILD_THRASH` to Pack Leader. Both Pack Leader and Dark Ranger AoE should use the state machine when Beast Cleave is talented.

Required ordering:

`Hero state -> Beast Cleave state -> Bestial Wrath admission -> Wild Thrash admission -> normal AoE priority`

Safeguards:

- Never let Beast Cleave fall off merely to wait for Bestial Wrath.
- Do not hold Wild Thrash if the pack will die before the Apex event matters.
- Re-read Beast Cleave after Bestial Wrath rather than assuming the new Apex pet inherited the old buff.
- Keep exact hold duration configurable / LIVE_VERIFY for haste, latency, pet travel and pack lifetime.

## Evidence grading

- SimC theoretical branch: HIGH confidence.
- Method practical timing concept: HIGH confidence.
- Exact hold duration and WCL adoption frequency: LIVE_VERIFY.
