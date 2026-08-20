# Unholy Death Knight — Dark Transformation / Blightfall GCD correction

Date: 2026-08-20
Status: STABLE_MECHANIC for Dark Transformation GCD semantics; runtime follow-up ordering remains LIVE_VERIFY.

## Change
SimulationCraft commit `c4df10535dd4812a786e000e0241520d0da30db6` corrected `dark_transformation_t` so `trigger_gcd = 0_ms` unconditionally, including when Blightfall is talented. The spell data contains a 1.5s GCD, but the commit clarifies that this is the shared GCD of the Blightfall replacement action and should not be charged to Dark Transformation itself.

## Rotation impact
- Dark Transformation remains off-GCD even with Blightfall talented.
- Blightfall is the GCD-consuming follow-up/replacement action.
- Do not model `Dark Transformation -> GCD locked -> Blightfall`; that inserts a false dead GCD and understates the burst window.
- This is an execution-policy correction rather than a rewrite of the Unholy priority list.

## Automation mapping
1. Admit Dark Transformation as an off-GCD cooldown.
2. Update transformation/replacement state immediately.
3. Evaluate Blightfall as its own GCD action.
4. Do not double-submit both actions into the same GCD slot if the runtime cannot safely sequence an off-GCD state change and replacement action in one tick.

## Cross-check
Current Wowhead Midnight Unholy material describes Blightfall as replacing Dark Transformation after the relevant rework and keeps Dark Transformation at the top of the normal priority, consistent with treating the transformation as a cooldown state transition rather than a GCD-consuming rotational strike. Public guides do not currently document the exact shared-GCD implementation detail, so runtime sequencing remains live-verify.
