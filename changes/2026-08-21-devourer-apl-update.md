# 2026-08-21 — Devourer Demon Hunter Season 2 APL update

SimulationCraft commit `304783bce59feed647af53be6867f52d558240f2` (`[Devourer] update APL`) materially changes Devourer execution. The update separates single-target and AoE Reap admission, raises AoE Reap requirements during `Moment of Craving`, removes old 10-soul Eradicate dump conditions, and removes Throw Glaive as a Void Metamorphosis filler in the affected VSM branch.

Key theoretical changes:
- Single target keeps the normal >=4 soul Reap cadence, with fight-end override at <=6s.
- AoE with Eradicate uses `4 + 6 * Moment of Craving` as the Reap threshold, so Moment of Craving pushes the threshold from 4 to 10 souls.
- Meta + Collapsing Star preparation now requires at least 4 souls before Reap contributes to the 30-stack setup.
- Eradicate in the VSM Meta branch is no longer triggered merely by >=10 souls consumed; it is tied more tightly to Moment of Craving expiry / Void Ray timing and Soulburst state.
- The prior Throw Glaive filler during high-Fury VSM Meta is removed; the APL may deliberately wait in 0.05s increments instead of spending a low-value GCD.

Public Wowhead 12.1 guidance (updated 2026-08-12) confirms the broader Devourer builder/spender framework around Fury, Soul Fragments, Void Ray, Reap, Void Metamorphosis and Collapsing Star, but does not yet document these exact new Season 2 AoE thresholds. Therefore the SimC branch is high-confidence as a theoretical benchmark, while the exact 4/10 soul practical thresholds remain `LIVE_VERIFY` pending high-end Season 2 raid/M+ logs and theorycrafter confirmation.

Automation impact: target count, Eradicate, Moment of Craving, Soulburst, Void Ray cooldown, Meta state, Collapsing Star stacks and set state must be evaluated before Reap admission. A short intentional wait inside Meta is now a legitimate state when a higher-value Void Ray window is imminent; the runtime should not force filler merely to avoid idle time.
