# 2026-08-23 — Shadow Priest practical APL refinement

SimulationCraft commit `924f24376fa051c295290bdad8ebb75b5f4bcd27` changes Shadow 12.1 execution in a player-practical direction rather than only optimizing a perfect robot timeline.

Key changes:
- Shadow Word: Madness target routing looks farther ahead (roughly a 2-GCD horizon) while cast admission still depends on actual expiry/resource/proc/Rift/Voidform conditions.
- Tentacle Slam charge/VT-refresh handling is evaluated earlier around Void Torrent windows.
- Void Blast is placed ahead of ordinary Mind Blast after higher-priority decisions.
- With Void Blast + Thought Harvester, ordinary Mind Blast can be held when Void Torrent is within roughly 2 GCD and DoT/setup state is ready; this is a short predictive hold, not a generic delay.

Current Method (12.1, updated 2026-08-11) and Wowhead (12.1, updated 2026-08-12) support the broad Voidweaver sequence: maintain DoTs and Shadow Word: Madness, use Void Torrent to establish Entropic Rift, prioritize Void Blast during the Rift, then use Mind Blast. They do not yet publish the exact new SimC two-GCD predictive gate as a universal player rule.

Automation recommendation:
- Separate target-selection horizon from cast-admission horizon for Shadow Word: Madness.
- Model HERO_STATE / ENTROPIC_RIFT_STATE before Void Blast vs Mind Blast decisions.
- Keep the ~2-GCD Mind Blast hold configurable and encounter-overridable for movement, target death, add timing and forced downtime.
- Mark exact timing as `LIVE_VERIFY`; do not convert it into a permanent hard hold until top live sequences support it.

Baseline added at `priest/shadow/baseline.yaml` and indexed in `library-index.yaml`.
