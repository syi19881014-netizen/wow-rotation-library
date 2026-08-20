# Unholy DK — Transfusion correction (2026-08-20)

## What changed
SimulationCraft commit `aa424a18d8557a35d8b0e8924d0e135fb850e0d4` briefly modeled Unholy Transfusion with `max_stack = 1` per pet. Six minutes later, follow-up commit `7ea02cb865fd4dd07a5be52d5bb6a1fe538c487e` explicitly removed that one-stack cap and instead set an 8-second Transfusion duration for non-Blood Death Knights.

## Correct current interpretation
- Unholy Transfusion is triggered across all active Lesser Ghouls by the current SimC San'layn execute-talent model.
- Each Unholy Transfusion application lasts 8 seconds.
- Do **not** hard-cap each Lesser Ghoul at one Transfusion stack.
- Repeated applications may overlap under the corrected SimC model; the exact live maximum stack ceiling remains `LIVE_VERIFY` because the public 12.1 talent text only states that Vampiric Strike increases the damage of the Lesser Ghoul it summons and does not expose a numeric stack cap.
- Blood retains asynchronous Transfusion stacking behavior and is not changed by this correction.

## Automation impact
This is a PET_STATE correction, not a player-priority rewrite. Runtime code should refresh/stack an 8-second Transfusion state on every active Lesser Ghoul on a qualifying trigger and must remove the temporary `stack <= 1` assumption. Do not add a player GCD, cooldown hold, or resource rule solely because of this correction.

## Evidence
- SimC `ab6ab283c995a809410dc31029a0066700178cdb`: applies Transfusion to all active Lesser Ghouls and removes the DK-side Epidemic Order proxy behavior.
- SimC `aa424a18d8557a35d8b0e8924d0e135fb850e0d4`: temporary one-stack implementation, now superseded for Unholy.
- SimC `7ea02cb865fd4dd07a5be52d5bb6a1fe538c487e`: removes the one-stack cap and sets Unholy Transfusion duration to 8 seconds.

Status: `PROVISIONAL_HIGH_CONFIDENCE` for exact live stacking ceiling; player core priority remains unchanged.
