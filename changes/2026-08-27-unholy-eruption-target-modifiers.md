# Unholy DK — plague eruptions ignore target damage-taken modifiers

Date: 2026-08-27
Status: PROVISIONAL_HIGH_CONFIDENCE / LIVE_VERIFY

## Change
SimulationCraft commit `0711f604384bf81fc724d869df3753e6c3326097` adds current-live bug behavior where Dread Plague/Virulent Plague eruption damage removes target damage-taken multipliers before the eruption resolves. The affected modeled paths are:

- Dread Plague Superstrain eruptions.
- Blightfall plague-consumption eruptions.
- Scourge Strike plague eruptions.

Normal periodic disease ticks remain separate and continue to use the ordinary target-modifier path.

## Practical implication
Soul Reaper applies a target debuff that increases damage from Unholy minions and diseases. Existing current guides deliberately time Soul Reaper before the later Blightfall, allowing normal plague ticks to spend most of the Soul Reaper window before Blightfall consumes the remaining duration. The new SimC behavior means the Blightfall/eruption portion must not be forecast as receiving the same target damage-taken amplification.

Do not create a new rule that rushes Blightfall earlier merely to place its eruption inside Soul Reaper or a boss vulnerability window. Preserve the established timing unless target lifetime, cooldown alignment, or another higher-value state requires an override.

## Automation
Split disease damage modeling into at least two event classes:

1. normal periodic plague ticks;
2. eruption/consumption damage.

For the current live-bug branch, do not multiply eruption forecasts by Soul Reaper or generic target damage-taken modifiers. Keep encounter-specific exceptions `LIVE_VERIFY` until log evidence confirms them.

## Evidence conflict / validation
Current Method and Icy Veins 12.1 guides still recommend Soul Reaper followed by a later Blightfall near the end of the Soul Reaper window. This is not necessarily contradicted by the SimC correction: delaying Blightfall allows more ordinary plague ticks to benefit from Soul Reaper before the non-amplified eruption consumes the remaining duration. Exact best timing remains a live-log question.
