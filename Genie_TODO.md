# Knight Genie Implementation Plan (TODO)

- [x] **Milestone 1 – Data model & packets**
  - [x] Define `GenieConfig` / `GenieRuntime` structs on server.
  - [x] Add `[GENIE]` defaults in `gameserver.ini` and clamp ranges/pcts.
  - [x] Introduce opcodes: `WIZ_GENIE` with subcmds CONFIG/TOGGLE/ACK.
  - [ ] Persist per‑user config (in‑memory; optional DB row `USER_GENIE_CONFIG`).
  - [x] Handler: validate/sanitize client config, store, send ACK.

- [ ] **Milestone 2 – Core loop (attack baseline)**
  - [x] Add `ToggleGenie` and `UpdateGenie(nowMs)` on `CUser`; hook into 400 ms genie thread.
  - [x] State machine skeleton: Idle/Working with recovery/support/attack slots.
  - [x] Target selection: nearest hostile within clamped `attackRange`; validate distance/LOS.
  - [x] Execute basic attacks (no skills yet); respect cooldown/rate limit (reuse `Attack` validation).

- [ ] **Milestone 3 – Skills & recover**
  - [x] Attack skill slots (4): server casts listed skills first, fallback to basic attack (validation via MagicProcess).
  - [x] Recover skill slots (4) + HP% threshold via self-cast skills; pet pot pending.
  - [x] HP/MP pot usage with cooldown, clamp by % thresholds.
  - [x] Auto-stop when dead or blinking (teleport); stun/para not flagged in GameDefine.h.

- [ ] **Milestone 4 – Support buffs**
  - [x] Support slots (12) iterate & recast every 30s (party flag not used on server; client decides).
  - [x] Support tick throttled to 2s, one support skill per tick.

- [ ] **Milestone 5 – Other triggers**
  - [x] Rule checks: low pots/arrows, party break, alone in region, durability==0 (server returns town/respawn and disables Genie). RepairThreshold/GenieHammer not implemented.
  - [x] Actions: ReturnTown/Respawn via NativeZoneReturn when triggers fire (repair state not implemented).

- [ ] **Milestone 6 – Auto-loot**
  - [x] Auto-loot: up to 2 pickups per tick within 8m, reusing ItemGet; respects weight/slots; no ownership timers yet.

- [ ] **Milestone 7 – Anti-cheat**
  - [x] Clamps: attack range validated per action; action interval enforced; pots already 900ms.
  - [x] Distance checked each attack; violations tracked and auto-disable after repeated range infractions (LOS unchanged).

- [ ] **Milestone 8 – Debug & admin**
  - [x] GM chat commands: /genieinfo [char], /genieoff [char] (manager authority only).
  - [ ] Structured logs still pending (only GM query so far).

- [ ] **Milestone 9 – Persistence & client sync**
  - [x] Store sanitized config in server cache; auto-load on login; (client send still needed for UI).
  - [ ] (Optional) Client UI wiring note: send config on change; display clamp/denial reasons.

- [ ] **Milestone 10 – Tests & rollout**
  - [ ] Unit tests for rule engine and config clamps.
  - [ ] Integration test harness for `UpdateGenie` with mocked time/targets/inventory.
  - [ ] Staged rollout flag: `GENIE_ENABLED` global + per-user disable.











