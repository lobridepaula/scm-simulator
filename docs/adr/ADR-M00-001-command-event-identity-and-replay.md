# ADR-M00-001 — Command/Event Identity, Ordering and Replay

**Status:** Proposed — **not accepted**. **Owner:** Software Architect. **Approver:** Product Owner (pending). **Issue/Story:** #3 / M00-S01 STORY_003 TL-002. **Baseline:** main `158d2773bc3ae40997185571a0c3319c619ea297`.

## Already binding (not a new decision)
[ARCHITECTURE §§4,6–8](../../ARCHITECTURE.md), [SPEC §11](../../SPEC.md), [INTERFACES](../architecture/INTERFACES.md): one authoritative GameState and writer per canonical domain, browser-independent engine, atomic command commit/reject, same command path for player and AI, deterministic issuer/command/event identifiers, stable event order, seeded PRNG streams, exactly 11 ordered daily phases, delivery floor day d+1. Domain effects must not be driven by independent event subscribers. No runtime implementation exists.

## Open decision and options
1. **ID namespace:** A — type-prefixed deterministic monotonically increasing counters stored per namespace in canonical state; B — namespaced deterministic hash of seed, type and allocated sequence; C — composite `scenarioId/day/phase/issuer/sequence`. All must avoid collisions across saves/closures, and avoid wall-clock/UUID randomness. **Selection pending**. Test collision prevention and replay; cross-version schema compatibility must be considered.
2. **Envelope and tie-break:** A — engine-allocated stable submission index and explicit phase/issuer/command-key tuple; B — per-issuer counters plus globally defined merge order. **Selection pending**. External/UI timestamps must not decide outcomes. Commands and resulting events require IDs, issuer, scenario day, payload discriminant, transaction reference and explicit rejection; exact field names and ordering tie-break await approval. Rejected commands cannot consume IDs inconsistently on replay.
3. **Canonical serialization / hashing:** A — versioned normalized JSON with sorted object keys, documented integer/string representation and excluded derived caches; B — deterministic binary canonical encoding with an explicit schema. **Selection pending**. Do not hash locale labels, DOM, caches or wall clock.
4. **Migration policy:** A — explicit sequential version migrations with invariant validation; B — reject all unsupported schema versions until migrations are independently tested. Both preserve pending orders, shipment, obligations, PRNG state, events and stable references. **Selection pending**.

## Domain dependencies
`ui/app -> application bridge -> simulation/engine -> domain public APIs -> simulation/shared`; adapters (storage, render, content) depend inward, never reverse. At orchestration boundaries, engine validates all projected changes against one consistent pre-commit state, then commits state, ordered events and deduplication keys atomically. Procurement owns orders; logistics shipments; inventory quantities/cost basis; finance funds/ledger; world lots/roads; economy companies/offers; production recipes; AI policy; learning evidence.

## Verification oracles (proposed, not executed)
- Duplicate command, event or posting ID => rejected without mutation or committed event.
- Reordering shuffled input maps into the documented stable key => identical state/event hash.
- Same seed, catalogs, versions and command sequence after save/load mid-transit => identical normalized events, balances, AI and KPI outputs.
- Invalid required envelope field, unknown issuer, stale day or nonexistent references => deterministic localized rejection with unchanged canonical state.
- Same normalized state with different object insertion orders => same hash; one changed authoritative amount => different hash.
- Illegal `src/simulation/**` import of React/DOM/IndexedDB, or domain-to-domain private import/cycle => architectural check fails.
- Tests must exercise all 11 phases and d+1 delivery minimum; no CI or numeric performance evidence exists.

## Consequences / rejected shortcuts
A adds simple explainability but requires persistent counters; B reduces allocation state but must define collision and re-derivation guarantees; C improves auditing but must handle cross-phase generation. Hash normalization is necessary regardless. Reject runtime UUID, wall-clock ordering, `Math.random()`, duplicate shadow state, and browser-dependent serializer semantics as incompatible with existing specification.

**Approval gate:** choose exact ID/envelope/hash/migration alternatives, capture PO decision and review with #2 save/financial custody requirements before promoting to Accepted or updating normative interfaces.
