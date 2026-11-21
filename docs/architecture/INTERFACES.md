# Internal Interface Contracts — Supply Chain Simulator

**Status:** Planned contracts; no exported TypeScript types, HTTP endpoints or browser adapter is implemented yet.

## Boundary inventory

| Contract | Provider → consumer | Rules and verification |
| --- | --- | --- |
| GameCommand | React/application adapter → engine command dispatcher | Typed, validated, stable IDs; atomic accept/reject; same pathway for player and AI |
| GameEvent / command result | Deterministic engine → projections, audit timeline, reporting | Ordered, traceable outcomes and localized rejection reasons; no side-effect duplication |
| Simulation state projections | Authoritative engine → dashboard, orders, map, facilities, directory, teaching | Read-only snapshots/selectors; no direct mutation of stock, ownership, orders or finance by UI |
| World/render projections | World/domain selectors → Canvas 2D renderer | Tile/chunk, road/lot spatial and overlay data; visual sprite positions are never economic truth |
| Scenario/catalog loader | Versioned local content → engine initialization | Validate sector/SKU connectivity, city feasibility, funding, seed and version compatibility |
| Save/load envelope | Engine state ↔ IndexedDB/local JSON adapter | Full replay-critical state, pending commitments, lifecycle data, schema version, atomic validation before restore |
| Report export | Canonical events/ledger/learning state → local JSON/CSV | Transparent KPI definitions and optional alias; never invent identity or derive unrelated figures |
| Asset manifest | Licensed local assets → rendering | Versioned visual IDs, license/provenance and a controlled missing-asset fallback |

## Error and compatibility requirements

- A rejected command returns an explicit reason and **must not** mutate authoritative state.
- B2B PO and SO are alternative views of one order; duplicate transport, payment or receipt is forbidden.
- State events and hashes are deterministically ordered by documented engine rules, including the eleven SPEC §11 daily phases.
- Serialization and import reject unknown required fields, invalid numeric/reference bounds, oversized payloads or unsupported versions with user-readable pt-BR messages.
- A save migration must be explicit and tested; never deserialize an arbitrary object directly into trusted simulation state.
- Contract/schema changes require impact assessment, integration fixtures and documentation updates before merging.

## Source contracts and unresolved details

- [ARCHITECTURE.md](../../ARCHITECTURE.md): allowed dependencies, authority, command/event ownership and interfaces.
- [SPEC §7](../../SPEC.md#7-purchase-orders-sales-orders-and-logistics): commercial order schema and lifecycle.
- [SPEC §11](../../SPEC.md#11-simulation-time-and-determinism): deterministic scheduling.
- [DATA_MODEL.md](DATA_MODEL.md): conceptual entities and persistence boundaries.
- [FEATURES.md](../../FEATURES.md): observable acceptance.

M00 must finalize the exact typed command/event envelope, stable ID format, rejection codes, schema locations and hashing/migration rules. Link actual source types here **only after they exist**. No backend/API endpoint is authorized by this document.
