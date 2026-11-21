# Supply Chain Simulator — Implementation Roadmap

**Document:** `ROADMAP.md`  
**Status:** Proposed  
**Planning baseline:** 2026-11-21  
**Target:** Complete classroom-ready **Release 1.0**, not an MVP  
**Source of truth:** [`SPEC.md`](./SPEC.md)  
**Execution checklist:** [`TODO.md`](./TODO.md)

## 1. Delivery strategy

Build the full product described by `SPEC.md` §§1–16 as **one integrated release**. Milestones M00–M08 are implementation and verification gates; **none is a separately scoped public product or MVP**. M09 authorizes Release 1.0 when all mandatory functionality and acceptance tests pass.

The architecture must accommodate the final specified system **from T0**: world generation; companies; lots and facilities; product catalogs; procurement and sales orders; stock and logistics; production; demand; AI competitors; finance; teaching; UI; persistence; and reporting. Domain contracts, data ownership, versioning, tests, and UI patterns are established early. Feature implementation remains incremental so that each change is testable, rather than attempting all systems simultaneously without integration.

**Scope boundary:** “Complete” means all requirements within the currently committed scope of `SPEC.md`, including the classroom and technical requirements. Capabilities explicitly excluded by §3.3 (such as multiplayer, tax/credit systems, perishable goods, and complex transportation) **are not secretly promised for Release 1.0**. Any decision to include them requires a versioned change to `SPEC.md`, acceptance tests, and this roadmap before work begins. The distinction between *not delivering an MVP* and *unbounded feature scope* is intentional.

**No stage-specific shortcuts:** Do not introduce fake transactions, cosmetic KPIs, infinite stock, nondeterministic AI, direct UI state mutation, or ephemeral save formats as temporary implementation substitutes. An unfinished system may have an interface and contract, but it must be explicitly unavailable until implemented; it must not simulate completion.

## 2. Target Release 1.0: definition of completeness

Release 1.0 supports: (a) reproducible **320 × 240 tile (76,800 tile)** city with district hierarchy, connected roads, variable-sized lots and four zones; (b) lot acquisition and lawful building/retirement of store/factory/farm/warehouse; (c) broad multi-category product networks, including guided fruit, cheese and furniture chains, with capacity-limited production/inventory; (d) authoritative B2B PO/SO workflows, shipments, receiving, settlement and internal transfers; (e) B2C retail demand, competition and pricing; (f) **at least 40 simultaneously accessible AI commercial counterparties** in standard presets, with deterministic founding, expansion, contraction, insolvency, liquidation and exit; (g) auditable financial ledger and KPIs; (h) full pt-BR interface, viewport-culling Canvas map, overlays and buyer/supplier directory; (i) 30-day teacher exercises, accessible workflows, local saves, reflection and CSV/JSON exports; and (j) CI plus static Vercel publication without login or paid services.

### Invariants that apply to every milestone

1. **Determinism:** `seed + generator/scenario versions + ordered commands = identical outcomes`, with named PRNG streams and stable processing order.
2. **Economic integrity:** inventory, commitments, cash, assets, revenue, and COGS derive from authoritative events; double-receipt and double-settlement are impossible.
3. **Clean boundaries:** TypeScript strict engine does not depend on React, browser storage, or rendering; the UI sends typed commands and reads projections.
4. **Traceability:** business commands, resulting events, and relevant financial entries are inspectable and testable.
5. **Extensibility without speculative frameworks:** data-driven catalogs, recipes, scenarios, AI policies, and versioned saves; avoid general-purpose infrastructure without a current use.
6. **Educational validity:** the learner sees what happened and why; metrics are explained and the teacher can assess reasoning, not profit alone.
7. **Accessibility and privacy:** pt-BR, keyboard operability, readable non-color status, no mandatory personal data, and safe import/export.
8. **Continuous quality:** typecheck, lint, unit and integration tests, and deterministic fixtures accompany changes; regressions block integration.

## 3. Architectural baseline and workstreams

Use `SPEC.md` §14 as the architecture baseline: React + Vite UI, independent TypeScript simulation engine, viewport-culled Canvas 2D city with local verified sprite assets, schema-validated JSON scenario data, IndexedDB/local JSON persistence, Vitest, browser E2E tests, and GitHub-based static deployment on Vercel. Keep internal names and technical documentation in English; player-facing content in pt-BR.

Workstreams run throughout the roadmap, even when a milestone is primarily focused elsewhere:

| Workstream | Owns | First established |
|---|---|---|
| Simulation kernel | Commands, domain events, deterministic scheduler, PRNG, projections | M00–M01 |
| Geography and ownership | 320×240 tiles, districts, road graph, lot footprints, land, facilities, map asset contract | M00–M02 |
| Economy and finance | Inventory, valuation, commitments, ledger, KPIs | M00–M03 |
| Supply chain | Orders, shipment custody, transfer, settlement | M00–M04 |
| Markets and operations | Extraction, manufacturing, consumer demand, prices | M00–M05 |
| AI and market ecology | Shared commands, 40+ counterparts, market entry/exit, firm lifecycle, competition | M00–M06 |
| User experience | Navigation, map, panels, localization, accessibility | M00 onward; integration M07 |
| Education | Curriculum evidence, missions, presets, teacher results | M00 onward; integration M08 |
| Persistence/quality/release | Versioning, fixtures, saves, CI, performance, publication | M00 onward; acceptance M09 |

## 4. Milestones

### M00 — Product contract and T0 foundations

**Objective:** eliminate ambiguity about the complete Release 1.0 and establish interfaces for all planned domains before feature implementation.

**Deliverables:** reconciled feature inventory and traceability to `SPEC.md` §§3–16; decisions for inventory costing, shipment ownership/payment timing, missing-business-rule cases and insolvency; architecture decision records; module dependency rules; TypeScript/React/Vite scaffold; schemas and version policy; CI baseline; UI information architecture/design tokens; pedagogical objective-to-event matrix; issue conventions.

**Exit gate:** all Release 1.0 capabilities are mapped to owners, interfaces, verification methods and future milestone IDs; project builds, typechecks and tests in CI; no unresolved decision blocks the domain model.

### M01 — Deterministic simulation platform

**Objective:** create the single authoritative, browser-independent runtime that all systems will use.

**Deliverables:** versioned scenario/catalog loaders, seeded PRNG streams, stable IDs, canonical game state, typed commands/events, domain validation, daily phase scheduler matching `SPEC.md` §11, event history, reproducibility test harness, frontend command bridge, initial persisted-state envelope.

**Exit gate:** identical seed/config/version/command streams produce identical state, event history and state checksum in tests; invalid commands leave state unchanged; browser UI cannot mutate internal state directly.

### M02 — City, lots, facilities and accessibility

**Objective:** complete the geographic and ownership rules that constrain the economy.

**Deliverables:** deterministic 320 × 240 tile world, arterial/collector/local road graph, shortest-path routing, four coherent zones, district demand clusters, variable-size non-overlapping purchasable lots, land price/accessibility indices, legal facility placement/ownership, starter lot, initial diverse 40+ viable AI company/facility network, chunkable map projections and accessible navigation.

**Exit gate:** multi-seed standard fixtures yield exactly 76,800 tiles, valid lots and zones, full facility-road access and initial 40+ trade-accessible AI firms; placement rejection is atomic; land values, routes and projections replay deterministically.

### M03 — Inventory, commitments and financial integrity

**Objective:** establish trustworthy stock and financial rules before transaction workflows depend on them.

**Deliverables:** versioned products and recipes; per-facility stock with cost basis, reservations and inbound capacity; cash commitments and available funds; asset accounting for lot/building purchases; typed ledger entries; expense/insolvency policy; revenue, COGS, net profit, ROI and operational metric projections.

**Exit gate:** property/inventory purchases do not artificially create P&L expenses; no resource balance or capacity is overcommitted; reporting reconciles to ledger and inventory valuations; negative profit is supported without negative cash.

### M04 — Procurement, sales orders and logistics

**Objective:** deliver the **complete order lifecycle** using M01–M03 rules, without special treatment for the player.

**Deliverables:** supplier offers, quote comparison including freight and lead time, single authoritative B2B order with PO/SO projections, status transitions, cancellation/rejection, seller stock and buyer fund/capacity reservation, dispatch, in-transit custody, delivery, receipt, settlement, internal stock transfers, shipment and order history.

**Exit gate:** full B2B and internal-transfer workflows pass automated integration tests, including simultaneous contention, failed paths, repeated receipt, cancellation and exact-once financial posting.

### M05 — Farm/factory operations, B2C demand and markets

**Objective:** close the operational loop from resources through production to household consumption.

**Deliverables:** extraction and harvesting, recipe-based production and lead-time availability, all three initial product chains, capacity and processing costs, warehouse functions, household demand clusters, accessibility-based retail competition, bounded prices, daily sales receipts, stockout and service-level indicators.

**Exit gate:** fruit, cheese and furniture work through valid supply chains; demand and stock limits are never exceeded; cost basis passes through production correctly; produced goods cannot be sold prematurely.

### M06 — Autonomous competitor economy

**Objective:** sustain a competitive, evolving multi-sector city with at least 40 viable AI counterparties available for the player to select as suppliers/customers.

**Deliverables:** multi-sector firm policies and offers/buyer demand; player and AI supplier/customer choice; fair contention; adaptive replenishment, pricing and production; endogenous company founding, lot acquisition, facility expansion/retrenchment, company distress, insolvency, liquidation, asset turnover and closure; funded entrant reserve maintaining the completed-day 40-counterparty minimum; auditable events and stable bounded decisions.

**Exit gate:** deterministic 30-day campaign preserves at least 40 viable, road-reachable AI counterparties at each completed day; verifies entering/exiting companies, asset/liability continuity, correct open-order handling, buyer/supplier access, shared rules and byte-identical replay.

### M07 — Complete player interface and interaction

**Objective:** expose every committed domain capability through a coherent, accessible application, not isolated debugging panels.

**Deliverables:** viewport-culling Canvas city view (320×240 default) with pan/zoom, lot selection, zone/ownership/competition/accessibility overlays, locally served licensed sprites, searchable/filterable directory of 40+ AI suppliers and customers, company lifecycle news, company/finance dashboards, purchase/sales/shipment forms, inventory and pricing panels, alerts, glossary and responsive pt-BR experience.

**Exit gate:** no mandatory action requires developer tools, raw JSON editing or map-only navigation; critical actions are operable with keyboard and pointer; financial and order displays match the engine.

### M08 — Complete classroom experience and local persistence

**Objective:** make the integrated game usable, reproducible and assessable in the intended course.

**Deliverables:** guided first purchase, learning objectives and hints; teacher preset selection/configuration, fixed-seed classroom runs, 30-day completion flow, decision timeline, optional reflection, assessment rubric evidence, pt-BR glossary, local IndexedDB save/load, JSON import/export, CSV results, schema migration/rejection strategy. Data and save contracts were defined at M00–M01 rather than deferred until now.

**Exit gate:** two independent browser sessions can run the same assigned scenario; a student can resume an in-flight shipment; reports reflect authoritative outcomes and explain financial differences; no mandatory student identity or hosted backend.

### M09 — Integrated verification, stabilization and Release 1.0

**Objective:** verify the **whole promised product**, then publish it.

**Deliverables:** complete `SPEC.md` §16 invariant suite; full 320×240 city and 40+ AI market seed/day-boundary coverage tests; entrant/closure and pending-order conservation regressions; browser E2E for all classroom steps; viewport FPS/memory and day-advance performance checks on school devices; asset license audit; accessibility/localization/privacy tests; CI and Vercel deployment.

**Exit gate:** every mandatory acceptance criterion passes; no critical/high-severity unresolved release blocker; full 30-day classroom path works without state editing; production URL is verified; downloadable data/report outputs work. **Only then** is the project called Release 1.0.

## 5. Dependencies and integration sequencing

```text
M00 Contract/architecture
  └─ M01 Deterministic platform
      ├─ M02 Geography & facilities ───────────────┐
      └─ M03 Inventory & finance ──────────────────┤
                                                  v
                                               M04 Orders & logistics
                                                  │
                                                  v
                                               M05 Production & B2C
                                                  │
                                                  v
                                               M06 AI economy
                                                  │
                    M00 UI/pedagogy foundations ──┤
                                                  v
                                               M07 Integrated UI
                                                  │
                                                  v
                                               M08 Classroom/persistence
                                                  │
                                                  v
                                               M09 Release verification
```

This diagram shows **gating dependencies**, not a prohibition on parallel work. For example, map visuals, teaching content, schema design, and test scenarios can evolve during M01–M06 provided domain contracts are respected. M02 and M03 may advance in parallel; M04 relies on both.

## 6. Verification and governance

A milestone is **Done** only when: its implementation is merged to the target branch; applicable automated tests and domain invariants pass in CI; its required user-facing path is operable or explicitly pending a later integration gate; acceptance evidence is linked to issues/PRs; changes to interfaces, scenario data, decisions or behavior are reflected in the authoritative docs; and there is no knowingly false/cosmetic behavior.

Track each checklist item in `TODO.md` as an independent GitHub issue when it represents reviewable code or documentation. Use IDs `Mxx-Tyy` in issue titles and PR descriptions. Milestone completion is a verification gate, not a percentage of tasks checked. Do not invent deadlines or story points before team capacity is known.

**Release-wide verification gates:** deterministic replay; inventory and monetary conservation/valuation; shared AI/player rules; inclusive and accessible controls; pedagogical traceability; offline/local persistence; privacy and safe input; reproducible build/deploy. A feature must not bypass a release-wide gate to satisfy a local milestone.

## 7. Documentation synchronization required before development

The committed Release 1.0 scope in `SPEC.md` includes the 320 × 240 world, multi-sector product catalog, minimum 40 AI counterparties, deterministic company lifecycle and asset management. Keep the issue backlog in `TODO.md` aligned to these requirements. `README.md` summarizes rather than redefining them.

1. Record architecture decisions for lot footprints, road/chunk data structures, lifecycle and order/asset liquidation semantics before implementation.
2. Treat AI founding and closure as committed work, **not optional expansion**.
3. Keep financial formulas, economic invariants, precise geography and lifecycle rules normative only in `SPEC.md`.
4. Maintain licensed asset provenance and test seed/performance baselines as implementation evolves.
5. Changes to capabilities or market-floor thresholds require coordinated specification, roadmap, TODO and acceptance test updates.


## 8. Release 1.0 final acceptance scenario

Run the ten-step end-to-end educational scenario in `SPEC.md` §16.2: initialize a fixed seed; choose cheese; compare suppliers; place a PO and inspect mirrored SO; track and receive the shipment; record household sales; explain profit versus cash versus ROI; purchase and build on an eligible lot; trade with an AI company; complete day 30 and export a verifiable report. Repeat with saved/reloaded state and with regression fixtures for fruit/furniture and competing AI orders. Any failure blocks Release 1.0.
