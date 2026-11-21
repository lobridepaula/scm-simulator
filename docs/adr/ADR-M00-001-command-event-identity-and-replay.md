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


### Ownership, allowed imports and public-contract inventory (documented target; not implemented)

This expands [ARCHITECTURE §4](../../ARCHITECTURE.md#4-layering-and-allowed-dependencies), [§6](../../ARCHITECTURE.md#6-state-ownership-and-domain-boundaries), [§7](../../ARCHITECTURE.md#7-commands-queries-events-and-atomicity) and [§11](../../ARCHITECTURE.md#11-commerce-inventory-shipments-and-finance). The entries below are **contract categories, not agreed exported TypeScript function signatures**. Cross-domain reads must go through an engine-provided immutable view or an explicitly versioned public interface; a domain cannot import or mutate a peer's private state.

| Logical owner | Canonical writer | Permitted direct imports/dependencies | Engine-facing public contract to define | Prohibited behavior |
| --- | --- | --- | --- | --- |
| `app/ui` | None for economic state | Application bridge, read-only map/report projections | Submit typed intent, render immutable view | Direct domain reducer/store writes |
| Application bridge | None for domain state | Public engine facade, selectors, external adapter interfaces | Queue/submit/advance/query/snapshot adapter | Reimplement simulation rules or keep shadow ledger |
| `simulation/engine` | Clock, queue, events, PRNG, transaction coordination | Domain **public** validation/transition contracts, `simulation/shared` | Validate, stage and atomically commit changes/events | Import DOM/React/IndexedDB; rely on asynchronous completion order |
| `world` | Geography, roads, zones, lots, access | `simulation/shared`; immutable reference inputs supplied by engine | Validate placement/access, propose world transitions, world projection | Mutate `economy` facilities or `finance` assets |
| `economy` | Firms, facilities, offers, demand, retail prices | `simulation/shared`; engine-supplied immutable views | Validate company/facility/offer lifecycle intents | Directly post cash or reserve inventory |
| `procurement` | Quotes, canonical B2B order and transfer order, lifecycle, linkage | `simulation/shared`; engine-supplied immutable availability views | Validate order/quote/transfer transitions and linkage | Create independent buyer/seller ledgers or alter peer holdings |
| `inventory` | On-hand, outbound/inbound commitments, capacity, cost basis | `simulation/shared`; engine-supplied immutable order/facility context | Validate quantity/capacity and stage stock-cost transitions | Directly settle cash or mutate order status |
| `production` | Recipe/extraction/production records | `simulation/shared`; engine-supplied immutable recipe/stock/finance views | Validate production capacity and request atomic inputs/outputs | Modify inventory store directly |
| `logistics` | Shipment, physical transit custody, arrival eligibility | `simulation/shared`; engine-supplied immutable road/order views | Validate route/shipment and stage custody transitions | Create independent order/finance authority |
| `finance` | Cash, commitments, assets, ledger, obligations | `simulation/shared`; engine-supplied immutable economic facts | Validate commitment/posting, stage idempotent ledger updates | Directly change inventory cost basis or shipment status |
| `ai` | AI policy state, planning/entry/liquidation progress | `simulation/shared`; immutable engine read models | Generate deterministic intents in company-ID order | Bypass shared player/AI command path or post directly |
| `learning` | Objectives, reflections, educational evidence | `simulation/shared`; immutable engine event/projection inputs | Record learning evidence and expose report projection | Recalculate canonical financial truth or mutate economy |
| `persistence/content/render` adapters | External storage/assets only; no simulation economic state | Application-facing public interfaces and immutable snapshots | Validate/load/save/render; rebuild derived caches | Be imported by `src/simulation/**` |

**Allowed graph:** `app/ui → application bridge → simulation/engine → domain public APIs → simulation/shared`; adapters depend inward on the public bridge/contracts and never the reverse. Every domain points only to `simulation/shared` through direct source imports. The engine may **call** public domain functions and pass immutable peer-domain views; this is not a permission for peer imports. Versioned content enters through a validated adapter. Canonical writes are committed by the engine using the owning domain's transition results. `simulation/shared` imports no higher layer.

**Negative architectural fitness fixtures (specified, NOT RUN):**
1. Add a synthetic import `src/simulation/finance/* → src/ui/*` or `react` or `indexedDB`: import-boundary check must fail.
2. Add a synthetic direct import `src/simulation/inventory/* → src/simulation/procurement/internalReducer`: module-private dependency check must fail.
3. Add synthetic cycles `world → economy → world` or `engine → adapter → engine`: directed import-graph acyclicity check must fail.
4. Add a synthetic UI call mutating cash/stock, or a logistics event listener posting a finance entry without engine authorization: ownership/command-path architecture test must fail; static analysis alone may not detect behavioral mutation.
5. Build a legal fixture in which engine coordinates procurement, inventory, logistics and finance through their public staged transitions: rejection leaves the entire pre-commit state unchanged and emits no committed events.
6. Use a shared deterministic simulation fixture with `Math.random()` and DOM access injected in the engine: forbidden-import and nondeterminism checks must fail.

Actual module imports, signatures, graph reachability and test outputs are **unverifiable** until implementation exists. No green CI, passed test or completed dependency proof is asserted by this inventory.

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

## Decision record and authority

- **Decision date:** Pending; this proposal has no approval date.
- **Selected alternative:** None — Product Owner decision pending.
- **Rejected alternatives:** None of the viable A/B/C alternatives rejected; only constraints explicitly prohibited by SPEC/ARCHITECTURE are invalid. Rejection rationale is pending the decision.
- **Supersedes:** None.
- **Decision log:** Proposed for M00-S01 STORY_003 / Issue #3; no accepted or superseded revision. Future approval must name the chosen option, approver, date, rationale, rejected alternatives and impacts.
- **Affected authorities:** `ARCHITECTURE.md` §§4,6–8,11,14,19; `SPEC.md` §§7,11,16; `docs/architecture/INTERFACES.md`; `docs/architecture/DATA_MODEL.md`; `docs/architecture/DECISIONS.md`. Exact public envelope/types and replay evidence require future PO decisions and implementation. The existing normative documents remain unchanged pending approved reconciliation.
- **Verification status:** **NOT RUN** — proposed oracles are specifications, not test definitions executed in CI; no application scaffold or runnable architecture check is present on the inspected baseline. Regression evidence, integration SHA and any later performance measurements must be recorded separately.
