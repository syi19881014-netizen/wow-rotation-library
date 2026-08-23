# Retribution Paladin — Hammer of Light / Crusade live bug (2026-08-23)

## Summary
SimulationCraft commit `e4fda8cc0de6415e65c1eb54433938db5442b7da` (2026-08-22 UTC) updates current live-bug modeling for Retribution Paladin: while Crusade/Avenging Wrath is active, Hammer of Light is modeled as granting **9 Crusade stacks**, rather than stacks based on its current 3 Holy Power cost. SimC applies this override only in `bugs` mode, so this must be treated as a live bug/quirk rather than intended permanent design.

## Practical evidence
- Wowhead 12.1 Retribution rotation guidance (updated 2026-08-17) says Crusade should be stacked sooner rather than later, but Execution Sentence should still be put on cooldown before obsessing over ramp speed.
- The same guide says Hammer of Light should normally be cast immediately after Wake of Ashes, with limited holds for a free proc around Avenging Wrath / Execution Sentence or another valuable buff state.
- Method 12.1 guidance likewise uses Wake of Ashes -> Hammer of Light in the Templar opener and describes Crusade as scaling from Holy Power spent during Avenging Wrath.

## Interpretation
The new mechanic does **not** justify a new bug-fishing opener by itself because the current practical opener already routes Wake of Ashes into Hammer of Light. The material automation change is state tracking: after Hammer of Light during Crusade, the runtime must refresh the actual Crusade stack count instead of predicting stacks from Holy Power cost.

## Automation rule
```text
if Crusade active
and Hammer of Light succeeds
and LIVE_BUG branch enabled:
    expect/observe a 9-stack Crusade jump
    immediately reread Crusade stacks
else:
    use normal spend-based stack semantics
```

## Safeguards
- Do not delay Execution Sentence solely to exploit the 9-stack behavior.
- Do not force every free Hammer of Light proc instantly; preserve existing hold rules around Wings/Execution Sentence/other high-value buffs.
- Keep this branch kill-switchable because Blizzard can hotfix it without changing the intended Crusade tooltip.

## Status
`PROVISIONAL_LIVE_BUG` — strong current SimC implementation evidence, practical guide flow is compatible, but no official Blizzard note confirms the 9-stack result as intended behavior.
