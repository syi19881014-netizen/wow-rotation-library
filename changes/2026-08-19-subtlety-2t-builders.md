# 2026-08-19 — Subtlety 2T builder and Secret Technique update

Source: SimulationCraft commit `8418766` / PR `#11751`, authored 2026-08-18 UTC.

Material changes:
- Shadowstrike is no longer the generic <=3-target Dance builder. The new APL uses Shadowstrike at <=2 targets without MID2 2pc, and only at 1 target with MID2 2pc; Shuriken Storm takes over aggregate-damage building above those thresholds unless priority-target routing explicitly overrides it.
- Deathstalker Secret Technique is now kept inside Shadow Dance. Trickster retains a narrow off-Dance exception when Unseen Blade is selected, Secret Technique effective cooldown duration is <18s, and Shadow Dance is not ready.
- Several Trickster finisher-spacing checks move from Secret Technique remains >=6s to >=3s.
- A Gloomblade Apex-fishing line is added at <=2 targets when Lingering Shadow has at least 35 stacks and the relevant Apex/Darkest Night/Supercharge/Secret Technique-ready gates are satisfied.

Public-guide conflict:
- Current Wowhead Midnight rotation guidance still describes Shadowstrike at 3 or fewer targets during Shadow Dance, so it is not yet synchronized with the Season 2 SimC builder split.
- Season 2 live logs are only beginning to accumulate; exact practical 2T and priority-target behavior remains LIVE_VERIFY.

Library action:
- Updated `rogue/subtlety/baseline.yaml` with explicit SET_STATE, TARGET_BRANCH, hero-talent Secret Technique admission, 3s finisher-spacing, and Apex-fishing branches.
