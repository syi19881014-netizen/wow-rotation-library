# Guardian Druid — 2026-08-23 APL fixes

## Evidence
- SimulationCraft commit `c5eca52d53213ee6ab38b4594ef6ec7754f48f21` (`[Guardian] Apl fixes`, 2026-08-23 UTC).
- The commit is synchronized from Dreamgrove commit `b1af759`, so the change is shared with the spec theorycraft source rather than being an isolated SimC-only guess.
- Current 12.1 Wowhead/Method Guardian guidance independently supports the same broad priorities: maintain Thrash, keep Mangle/Thrash productive, treat Gory Fur as a bidirectional Ironfur/offensive-spender loop, and prioritize Ravage/DoTC apex windows while preserving a defensive rage reserve.

## Material execution changes
1. Free Gory Fur Ironfur is now an immediate off-GCD priority.
2. Ordinary missing-Ironfur maintenance lines with Killing Blow are suppressed while Ravage is available; survival overrides still win in real tanking.
3. Stack/refresh-aware Thrash maintenance is restored. Target stack caps are 3 baseline, 4 with Flashing Claws rank 1, 5 with rank 2.
4. Druid of the Claw + Fount of Strength raises Mangle priority while Answered Calling / apex spirit is active, including ST and Berserk windows up to 4 targets.
5. The Killing Blow + Answered Calling offensive Maul rage threshold falls from 90 to 80 in the theoretical APL; keep this configurable because real incoming damage can require a larger Ironfur reserve.
6. Wild Guardian is admitted with Lunar Beam only when Answered Calling / apex spirit is not already active, preventing it from being wasted during the apex-spirit window.

## Production interpretation
- This is a partial production-priority rewrite, not formatting.
- SimC/Dreamgrove conditions are a DPS benchmark; defensive survival reserve remains a higher-level runtime override.
- The runtime should read Gory Fur free-Ironfur, Ravage availability, Thrash stacks/refreshability, and Answered Calling before deciding Ironfur/Mangle/Wild Guardian.
- Do not blindly hard-code the 80-rage offensive threshold as a tank survival rule.
