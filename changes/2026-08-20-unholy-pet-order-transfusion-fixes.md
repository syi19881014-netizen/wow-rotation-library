# Unholy DK 12.1 — pet/order/transfusion fixes (2026-08-20)

## Change summary
SimulationCraft commits `ab6ab283c995a809410dc31029a0066700178cdb` and `aa424a18d8557a35d8b0e8924d0e135fb850e0d4` incorporate Blizzard-side Unholy behavior fixes that materially change pet-state modeling but do not yet justify a player-facing priority rewrite.

## Confirmed model corrections
- Epidemic Order is executed by the Lesser Ghoul pet rather than a DK-side proxy action. Remove the previous proxy-side Rune of the Apocalypse trigger assumption.
- A qualifying San'layn execute-talent trigger now applies Transfusion across all active Lesser Ghouls. For Unholy, each ghoul's Transfusion is capped at one stack; Blood keeps asynchronous stacking behavior.
- The old Lord of the Dead Magus Frostbolt shortened-GCD bug model has been removed.

## Practical interpretation
Current 12.1 design already defines Army of the Dead Scourge Strike as commanding nearby ghouls into Death Order or Epidemic Order based on nearby enemy count; the player does not directly choose which Order fires. These fixes therefore belong primarily in pet/state simulation and runtime forecasting, not in the ordinary GCD priority list.

## Status
- Pet-side Epidemic Order ownership/proxy removal: STABLE_MECHANIC.
- Unholy Transfusion all-ghoul application and one-stack-per-ghoul semantics: PROVISIONAL_HIGH_CONFIDENCE pending live-log/theorycrafter confirmation.
- Lord of the Dead Magus Frostbolt timing: PROVISIONAL_HIGH_CONFIDENCE pending current Season 2 cast-sequence validation.
- Core player rotation rewrite: false.
