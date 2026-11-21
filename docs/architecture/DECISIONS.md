# Architecture Decisions — Supply Chain Simulator

**Status:** Working decision index; the application is not yet implemented.

## Purpose and authority

Record significant architecture tradeoffs, rejected alternatives and migration consequences. [ARCHITECTURE.md](../../ARCHITECTURE.md) defines the normative **current target structure**; [SPEC.md](../../SPEC.md) owns functional/economic policy. A decision entry cannot override either document without an approved coordinated amendment.

## Current baseline

The following choices are **already specified**, not new decisions made by creating this index:

- Modular, domain-oriented TypeScript monolith with React/Vite UI and independent simulation runtime — ARCHITECTURE §§2–4.
- Single canonical simulation state and typed commands/events; deterministic eleven-phase scheduler — SPEC §11 and ARCHITECTURE execution sections.
- Browser-local IndexedDB saves and validated JSON exchange; static Vercel deployment, no mandatory backend — ARCHITECTURE §§3, 14–18.
- Viewport-culled Canvas 2D map, local licensed assets, fixed default 160 × 120 tiles — SPEC §4 and ARCHITECTURE §9.

## Decision backlog (not approved ADRs)

Before dependent work is completed, resolve the six decisions in [ARCHITECTURE §19](../../ARCHITECTURE.md#19-architecture-decisions-requiring-m00-closure):

1. Inventory costing/valuation and rounding.
2. Ownership, custody and transfers during shipping and liquidation.
3. Insolvency, liquidation and treatment of pending obligations/assets.
4. ID namespaces, command/event envelope, canonical serialization and migration.
5. Road graph representation, cached routes and renderer chunk sizing based on benchmarks.
6. Pre-day command queue editing and validation semantics.

Related execution work: [TODO M00-T03–T05](../../TODO.md#m00--product-contract-and-t0-foundations).

## Record format and approval

For each actual decision record capture: stable ADR ID; title; status (Proposed/Accepted/Superseded); date and approver; question/constraints; considered options; rationale; consequences; effects on SPEC/FEATURES/ARCHITECTURE; tests; superseding link.

**Placement reconciliation:** ARCHITECTURE currently identifies individual ADRs under docs/adr/. Retain that existing convention until the Product Owner approves a change; this document is only an index and creates no competing ADR folder.

## Change process

Propose → assess requirements and invariant impacts → obtain Product Owner approval where gameplay or major structure changes → update authoritative documents → implement and verify → record decision history. Never mark a listed open question as Accepted merely because documentation was generated.

See [FEATURES.md](../../FEATURES.md) and [ROADMAP.md](../../ROADMAP.md) for acceptance and scheduling.

## Proposed ADR package for M00-S01 STORY_003 (not accepted)

The following **proposal-only** records elaborate the existing backlog; they do not alter `SPEC.md`, `ARCHITECTURE.md`, `INTERFACES.md`, or the status of Issue #3:

- [ADR-M00-001 — Command/event identity, ordering and replay](../adr/ADR-M00-001-command-event-identity-and-replay.md): alternative ID namespaces, envelope ordering, canonical hash and migrations; **Proposed**.
- [ADR-M00-002 — Pre-day queue and atomic transactions](../adr/ADR-M00-002-pre-day-queue-and-transactions.md): editable queue and phase-1 validation alternatives; **Proposed**.
- [ADR-M00-003 — Spatial index, routing and renderer chunks](../adr/ADR-M00-003-spatial-index-routing-and-rendering.md): measurement-dependent alternatives; **Proposed**, no benchmark results.
- [ADR-M00-004 — Finance/custody/liquidation interfaces](../adr/ADR-M00-004-finance-custody-and-liquidation-interfaces.md): cross-review with Issue #2 and Product Owner; **Proposed**.

**Dependency order:** independent ADR option/owner review → Product Owner decisions (G2-arch and G2-fin) → serialized edits to shared normative contracts → automated architecture/engine tests and independent QA → Story/Issue acceptance. The documentation-only G1 traceability baseline was integrated via PRs #86/#87, but STORY_001 still records partial gate acceptance; this is not treated as permission to finalize blocked decisions.
