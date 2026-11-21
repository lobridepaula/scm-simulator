# Data Model — Supply Chain Simulator

**Status:** Conceptual domain catalog for target Release 1.0; not an implemented database schema.

## Data authority

The [SPEC.md](../../SPEC.md) defines semantics, economic invariants and quantities. [ARCHITECTURE.md](../../ARCHITECTURE.md) owns state/component boundaries. [FEATURES.md](../../FEATURES.md) describes required user-visible behaviors. This document catalogs conceptual entities and their relationships, without redefining those contracts.

## Canonical entity groups

| Concept | Responsibility / key relationship |
| --- | --- |
| Scenario / SimulationState | Seed, version identifiers, current day, deterministic PRNG state, ordered commands/events, active and historical records |
| Tile / District / Road graph | Geographic cells, district demand/accessibility aggregates and traversable road edges |
| Lot | Immutable lot ID, contiguous footprint, zone, road access, owner, price and optional facility; one operating facility per lot |
| Company | Stable identity, player/AI kind, lifecycle, owned properties/facilities and obligations; closed firms remain in history |
| Facility | Store/factory/farm/warehouse; one company owner and legal lot; capacity, status, supported operations and operating costs |
| Product / Sector / Recipe | Versioned SKU, unit, category, permitted sources/buyers, production inputs/outputs and cost parameters |
| Inventory / Reservation | Stock per facility/product, committed outbound quantities, inbound capacity and cost basis |
| Offer / Procurement request / Quote | Seller offers and AI buyer requirements, validity and acceptance before a canonical order is created |
| CommercialOrder / TransferOrder | One cross-company B2B record exposed as PO and SO views; intra-company transfer carries stock without consolidated sales |
| Shipment / Goods receipt | In-transit custody, road route, estimated arrival and once-only delivery/receipt |
| Financial ledger / Equity / Asset | Transaction postings, cash and reserved funds, operating income/costs, ownership transfers and derived KPIs |
| Consumer demand / AI policy | District-level demand and deterministic, bounded competitor behavior and entry/exit proposals |
| Learning objective / Report | Classroom objectives, scenario event evidence, rubric measurements and local exports |

## Invariants

- IDs remain stable across saves, closure, liquidation and historical reports.
- Every canonical entity has one authoritative owner; UI and reports are projections, not alternative ledgers.
- Company, facility and lot are separate entities. Lot closure cannot erase title or commercial history.
- Stock, reserved commitments, available-to-spend cash, inbound capacity and ledger postings cannot silently duplicate or go negative in prohibited contexts.
- Purchase/sales orders are projections of a **single** commercial order; its receipt and settlement are idempotent.
- Numeric money uses integer BRL cents. Inventory costing policy remains an explicit M00 decision, not an implicit schema choice.
- Entity references, product feasibility, road reachability and save-format versions require validation.

## Storage, migrations and privacy

- **Simulation system of record:** in-memory, canonical deterministic state owned by the engine.
- **Planned local storage:** IndexedDB save snapshots plus explicitly validated JSON import/export and local CSV/JSON reports. No server database is specified.
- **Versioning:** include generator, scenario, product catalog and save/schema versions; replay needs PRNG streams, events and pending commitments. Reject incompatible saves clearly unless a tested migration exists.
- **Data protection:** no mandatory student identity, account, remote profiles or school records; voluntarily entered aliases are not a required key.

## Data flow

Versioned scenario/catalog → validated initialization → typed player/AI command → engine validation and deterministic ordered events → authoritative state/ledger → read-only UI/map/education projections → local save/report adapter.

Detailed command/projection contracts live in [INTERFACES.md](INTERFACES.md). Pending choices are indexed in [DECISIONS.md](DECISIONS.md). Do not implement schemas from this conceptual table without confirming SPEC/ARCHITECTURE and M00 decisions.
