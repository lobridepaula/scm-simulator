# Supply Chain Simulator — Implementation TODO

**Document:** `TODO.md`  
**Status:** Proposed; no implementation tasks marked complete  
**Planning baseline:** 2026-11-21  
**Release target:** Complete **1.0**, not an MVP  
**Milestones:** [`ROADMAP.md`](./ROADMAP.md)  
**Requirements:** [`SPEC.md`](./SPEC.md)

## 1. How to execute this backlog

Checklists are ordered primarily by dependency. IDs `Mxx-Tyy` are stable references for GitHub Issues, pull requests, tests and review notes. An item is complete only when its behavior is implemented, tests pass, acceptance evidence is available, and relevant authoritative documents have been updated. A checked task is **not** permission to ship a partial release. Keep all work in the integrated architecture, with no temporary rule systems.

**Suggested issue metadata:** milestone ID; task ID; requirement section(s); owner; dependencies; acceptance tests; PR link; status (`todo`, `in progress`, `blocked`, `done`). Subtasks inside a PR should not multiply into issues unless independently reviewable.

**Global Definition of Done:** `tsc --noEmit`/typecheck, lint, tests and production build pass; no browser dependencies inside the simulation; data changes are schema-versioned; deterministic examples still replay; pt-BR strings are localized; failures have useful user-facing messages; accessibility and documentation impact are evaluated.

## M00 — Product contract and T0 foundations

- [ ] **M00-T01** Inventory every committed Release 1.0 requirement in `SPEC.md` §§3–16 and map it to milestone, module, acceptance test and observable learning outcome; distinguish mandatory, SHOULD, optional and excluded scope.
- [ ] **M00-T02** Reconcile `SPEC.md` §17 and `README.md` with the **single complete Release 1.0** strategy; link `ROADMAP.md` and `TODO.md`; avoid “MVP,” “first playable release,” or separate partial releases as delivery promises.
- [ ] **M00-T03** Record decisions for bounded contexts, authority/state, typed commands/events, the **11-phase scheduler**, company lifecycle/insolvency and liquidation, 320×240 lot/road/chunk data structures, PRNG streams and projected views.
- [ ] **M00-T04** Resolve accounting and logistics ambiguities **before coding**: inventory costing method; asset capitalization and valuation; custody/title during transit; exact recognition/payment boundaries; cash reservation and freight payer; handling pending obligations and insolvency; idempotency keys.
- [ ] **M00-T05** Define product/scenario/save/report schema versions, migration/rejection rules, broad connected product catalog, funded entrant profiles, 40-counterparty market floor, teacher presets and scenario feasibility validation.
- [ ] **M00-T06** Establish React + Vite + strict TypeScript project structure matching `SPEC.md` §14, with domain/UI/persistence isolation and path aliases if justified.
- [ ] **M00-T07** Configure package scripts (`dev`, `build`, `typecheck`, `lint`, `test`, `test:e2e`), formatting conventions, GitHub CI, protected review practices and deterministic test seed fixtures.
- [ ] **M00-T08** Define UI architecture, 320×240 Canvas viewport/chunk renderer and local asset manifest/provenance contract, supplier/customer company directory, overlays, pt-BR localization, keyboard/focus/screen-reader behavior.
- [ ] **M00-T09** Define pedagogical objective → game action/event → report evidence → rubric mapping, including a fixed-seed cheese assignment and alternative fruit/furniture scenarios.
- [ ] **M00-T10** Write architectural dependency checks and milestone acceptance test plan; document nonfunctional target measurements for classroom devices instead of assuming arbitrary performance thresholds.

**M00 exit:** every mandatory system has a domain owner/interface, verification path and milestone; CI runs on scaffold; unresolved questions cannot silently change accounting or domain invariants.

## M01 — Deterministic simulation platform

- [ ] **M01-T01** Implement schema validation for versioned scenario/config/catalog files and produce clear errors for unsupported or malformed data.
- [ ] **M01-T02** Implement seed normalization, independent deterministic PRNG streams (world, demand, AI, events) and stable ID generation; ban outcome-critical `Math.random()` and wall-clock dependence.
- [ ] **M01-T03** Implement normalized authoritative `SimulationState` and read-only domain projections; define ownership of lots, facilities, stock, commitments, orders, shipments, AI policies and ledger.
- [ ] **M01-T04** Define and validate typed player/AI `GameCommand` and domain `GameEvent` contracts, including rejection reason codes and stable event ordering.
- [ ] **M01-T05** Implement command dispatcher as an atomic validation/apply boundary; failed commands must not mutate state or emit settlement events.
- [ ] **M01-T06** Implement daily scheduler with the exact ten phases in `SPEC.md` §11 and documented conflict tie-breakers; ensure production outputs cannot join earlier same-day dispatch.
- [ ] **M01-T07** Build event timeline, deterministic state snapshot/checksum and replay harness using seed + versions + ordered commands.
- [ ] **M01-T08** Create UI-to-engine command adapter and state projection selector layer; no direct simulation mutation in React components.
- [ ] **M01-T09** Establish versioned save envelope and serialization boundary now, including pending actions and PRNG state, even though full browser persistence UI arrives later.
- [ ] **M01-T10** Add unit and property/regression tests for reproducibility, command atomicity, phase order and invalid-state rejection.

**M01 exit:** independent test runs generate identical snapshots, events and hashes; invalid commands cause no state change.

## M02 — City, lots, facilities and accessibility

- [ ] **M02-T01** Define and validate generator version/config defaulting to **320 × 240 (76,800 tiles)**, road hierarchy, districts, zones, variable-size lot footprints, starting firms/facilities and reproducible seeds.
- [ ] **M02-T02** Implement deterministically seeded arterial/collector/local road graph, connected access nodes and cached shortest-path distance/routing; reject unreachable firms/shipments.
- [ ] **M02-T03** Generate four zones, coherent economic districts, residential demand clusters, non-overlapping contiguous lots of varied footprints and legal access to all active establishments.
- [ ] **M02-T04** Calculate accessible-demand/footfall indices and nonnegative land prices from documented scenario-configured parameters.
- [ ] **M02-T05** Define immutable lot IDs, footprints, owner/asset records, zoning/access/collision constraints, purchase and redevelopment eligibility, one operating facility per lot, and recorded transfer on firm closure.
- [ ] **M02-T06** Implement store/factory/farm/warehouse base entities with ownership, parent lot, status, capacity, operating cost and allowed operations.
- [ ] **M02-T07** Provision player starter store and **40+ distinct trade-accessible AI companies** across connected product categories, each with legal sites, documented startup equity, road access and viable offer/request profiles; reject nonviable seeds.
- [ ] **M02-T08** Provide chunkable Canvas 2D viewport projections, spatial lot/facility indices, stable visual asset IDs and company/facility list navigation independently of the map.
- [ ] **M02-T09** Test multiple 320×240 seeds for 76,800 tiles, all zones, contiguous/non-overlapping lot footprints, valid road access for 40+ firms, atomic placement rejection, deterministic lot values and paths.

- [ ] **M02-T10** Benchmark full-map generation, route queries and spatial indices on representative classroom hardware; record baseline memory and time budgets.

**M02 exit:** valid reproducible 320×240 cities, 40+ viable starting AI counterparts, legal ownership/placement and efficient spatial projections.

## M03 — Inventory, commitments and financial integrity

- [ ] **M03-T01** Define integer-unit products, standardized packaging, product eligibility and initial catalogs for fruit, milk, cheese, wood and furniture, with all recipe/source prerequisites.
- [ ] **M03-T02** Implement facility/product stock balances: `onHand`, `reservedOutbound`, `reservedInboundCapacity`, stock value/cost basis and in-transit custody linkage.
- [ ] **M03-T03** Implement atomic stock/capacity reservation, commitment release and available-to-promise calculations; prevent double reservation.
- [ ] **M03-T04** Implement cash/equity balances, earmarked purchase-plus-freight commitments and available-to-spend validation across every payment-capable command.
- [ ] **M03-T05** Implement financial ledger with idempotent posting keys and entries for equity, land/building assets, stock acquisitions, sales, COGS, freight and expenses.
- [ ] **M03-T06** Implement lot/building asset purchases as cash-to-asset changes, never immediate operating losses; verify accounting reconciliation.
- [ ] **M03-T07** Implement expenses, unpaid obligations, financial insolvency/suspension and reasons; expose status as authority for the later company lifecycle/liquidation workflow, without negative cash.
- [ ] **M03-T08** Implement ledger-derived revenue, simplified net profit, ROI, cash, assets and operational KPI projections with explicit period/denominator metadata.
- [ ] **M03-T09** Test money arithmetic in BRL cents, rounding, stock valuation, negative profit, reserved funds, reconciliation and no double posting.

**M03 exit:** accounting and stock invariants hold independently of UI and simulated commercial order workflow.

## M04 — Procurement, B2B sales, shipments and internal transfers

- [ ] **M04-T01** Implement supplier offer publication, purchasability checks, comparative landed-cost/freight/lead-time quotes and offer validity.
- [ ] **M04-T02** Define the single canonical B2B commercial order with mirrored buyer PO and seller SO projections, auditable identifiers and authoritative status history.
- [ ] **M04-T03** Implement `draft`, `submitted`, `confirmed`, `rejected`, `cancelled`, `dispatched`, `in_transit`, `delivered` state transitions with legal guards.
- [ ] **M04-T04** Validate buyer/seller/facility/product ownership, quantities, available stock, available uncommitted cash, destination capacity and connected route before confirmation.
- [ ] **M04-T05** Reserve seller outbound stock, destination inbound capacity and buyer total purchase-plus-freight funds once per confirmed order.
- [ ] **M04-T06** Implement dispatch from seller custody, shipment creation, shortest-road distance, deterministic travel time and freight pricing; minimum next-day delivery.
- [ ] **M04-T07** Implement exactly-once receipt, destination inventory valuation including landed freight, seller revenue/COGS and buyer/seller settlement at successful delivery.
- [ ] **M04-T08** Implement rejection/pre-dispatch cancellation and exact-once commitment release; prevent post-dispatch cancellations not supported by the spec.
- [ ] **M04-T09** Implement intra-company transfer orders with physical movement, cost carry-over and no consolidated company revenue/COGS.
- [ ] **M04-T10** Build integration tests for contention over stock, cash and capacity; unreachable paths; duplicate receipts; cancellation; order replay; and player ↔ AI / AI ↔ AI symmetry.

**M04 exit:** every supported trade/transfer follows auditable, value-preserving physical and financial events.

## M05 — Production, extraction, household demand and retail

- [ ] **M05-T01** Implement broad multi-category connected product/recipe catalog, permitted sources/buyers, facility permissions, input/output quantities, daily limits and costs, while preserving guided fruit/cheese/furniture chains.
- [ ] **M05-T02** Implement farm production/resource extraction with output capacity, cash/operating cost and legitimate inventory valuation.
- [ ] **M05-T03** Implement factory production with exact input consumption, processing cost capitalization, no negative stock, and outputs eligible only after the proper scheduler boundary.
- [ ] **M05-T04** Implement warehouse storage, dispatch and transfer flows using the same inventory/capacity rules; no manufacturing or household sales there.
- [ ] **M05-T05** Configure end-to-end fruit, cheese and furniture recipes and establish feasible seed scenarios for each.
- [ ] **M05-T06** Implement seeded household product demand quotas, residential market reachability and bounded price/availability-based allocation among stores.
- [ ] **M05-T07** Implement player/AI store retail pricing, consumer receipts, external revenue/COGS recognition and stock decrement; never exceed cluster demand or store availability.
- [ ] **M05-T08** Add stockout, missed demand, facility utilization, service and inventory-value metrics, derived from authoritative simulation events.
- [ ] **M05-T09** Test complete farm→store and producer→factory→store→consumer paths, production limits, cash sufficiency, valuation flow and demand conservation.

**M05 exit:** all committed product chains operate end to end with valid financial and inventory consequences.

## M06 — Autonomous competitors and competitive markets

- [ ] **M06-T01** Implement **40+ independent initial AI counterparties** and configurable deterministic sector-aware strategies (stock, price, supplier/customer choice, reliability, expansion, closure and risk tolerance).
- [ ] **M06-T02** Ensure AI submits the **same typed commands** as the player and passes identical funds/stock/capacity/ownership rules.
- [ ] **M06-T03** Implement AI offer publication, order confirmation/rejection, replenishment and purchasing from either AI or player suppliers.
- [ ] **M06-T04** Implement production planning, stock-aware retail pricing, household market competition and response to shortages.
- [ ] **M06-T05** Implement stable company-ID decision/tie-break order, cadence-based bounded AI and market ecology evaluation, indexed offers/districts, dedicated entry PRNG stream and no future event knowledge.
- [ ] **M06-T06** Verify complementary and competing supply networks, including competitor-as-customer, competitor-as-supplier and AI-to-AI purchase chains.
- [ ] **M06-T07** Run 30-day seeded campaigns of 40+ AI firms and assert valid trades, outcomes, market-floor maintenance, reproducible histories, funded entrants, no unrecorded resource creation and no deadlocks.

- [ ] **M06-T08** Implement firm lifecycle states, deterministic entry scoring (unmet demand, margin, competition, route, available lots and funded equity) and explicit founding events.
- [ ] **M06-T09** Implement expansion, new facility purchase, retrenchment, closure decisions and legal property/asset ownership transitions using ordinary typed commands.
- [ ] **M06-T10** Implement distress, insolvency, liquidation, pending-order/cash/stock/shipment resolution, idempotent closure and historical company retention.
- [ ] **M06-T11** Implement viable reserve entrant pipeline enforcing >=40 road-reachable, selectable AI counterparties at initial and completed-day boundaries; report infeasible presets instead of faking resources.
- [ ] **M06-T12** Test company churn, order/ledger conservation, asset transfer, replay after save/load, market access after exits, entrant capital traceability and bounded performance.

**M06 exit:** 40+ viable and trade-accessible AI firms coexist with reproducible entry/exit, legitimate liquidation and accessible supplier/customer choices.

## M07 — Complete UI and player interaction

- [ ] **M07-T01** Finalize the app shell, navigation, loading/error/empty states, localized status messaging and complete pt-BR labels/glossary hooks.
- [ ] **M07-T02** Build viewport-culled/chunked Canvas 2D rendering for 320×240 tiles; pan/zoom, road/district/lot/owner/facility visibility, zone/land-value/accessibility/competition overlays, selection and valid construction.
- [ ] **M07-T03** Build searchable/list-based facility navigation and ownership/zone/capacity/price/production panels accessible independently of the map.
- [ ] **M07-T04** Build complete Orders experience: supplier comparison, draft/submit, PO/SO mirrored views, transfers, shipment timeline, cancellation and localized rejection explanations.
- [ ] **M07-T05** Build Company Dashboard: revenue, simplified profit, ROI formula breakdown, cash, trends, expense/commitment alerts and facility portfolio.
- [ ] **M07-T06** Build searchable directory for 40+ AI buyers/suppliers, firm status/history and competitive offers; compare landed cost and ETA alongside inventory, committed stock, recipes, replenishment and price actions.
- [ ] **M07-T07** Integrate pause/advance day, optional multi-day confirmation, event/day summary and actionable insolvency or stockout explanations.
- [ ] **M07-T08** Run keyboard, focus visibility, contrast, reduced-motion, non-color coding and desktop/tablet responsive audits on critical player paths.
- [ ] **M07-T09** Add browser E2E coverage for every major screen and player action; assert UI results against authoritative engine state rather than mocked numbers.

**M07 exit:** full game operation is available through accessible UI without developer intervention.

## M08 — Education, local persistence and teacher assessment

- [ ] **M08-T01** Implement first-session guidance, first PO walkthrough, contextual glossary, optional hints and progressive learning objectives without altering business rules.
- [ ] **M08-T02** Implement teacher presets with fixed seed/config versions, capital, product choice, starting networks, permitted actions, learning targets and evaluation rubric.
- [ ] **M08-T03** Implement the 30-day scenario completion state and learner feedback connecting decisions to inventory, service and financial outcomes.
- [ ] **M08-T04** Implement optional learner reflection/justification fields and time-ordered decision/event evidence suitable for teacher review.
- [ ] **M08-T05** Implement robust IndexedDB save/load with versioned snapshots, current day, PRNG states, pending commitments, shipments, financial ledger and AI state.
- [ ] **M08-T06** Implement safe versioned JSON save import/export, strict schema/size checks and explicit migrations or useful incompatibility errors.
- [ ] **M08-T07** Implement CSV and JSON final reports with scenario identification, seed, versions, orders, performance KPIs, learning objectives, decision history and optional aliases.
- [ ] **M08-T08** Implement proposed rubric evidence mapping (order process, inventory choices, financial interpretation, justified decision); do not claim official BNCC certification.
- [ ] **M08-T09** Test full scenario resume during transit/production; fixed-seed teacher comparison; report consistency after reload; privacy/no-account requirements.

**M08 exit:** the planned lesson can be assigned, resumed, completed, assessed and exported entirely locally.

## M09 — Release-wide validation and publication

- [ ] **M09-T01** Automate every invariant in `SPEC.md` §16.1 with sufficient seed/command fixtures, including exactly-once events and financial-vs-inventory reconciliation.
- [ ] **M09-T02** Automate the full ten-step teacher scenario in `SPEC.md` §16.2 using a fixed cheese seed, plus regression scenarios for fruit and furniture.
- [ ] **M09-T03** Exercise 30-day campaigns on 320×240 cities with 40+ AI firms, entrant and closure events, insolvency, pending shipments, invalid inputs and deterministic replay; assert no duplicate IDs or invalid state.
- [ ] **M09-T04** Audit keyboard/screen-reader experience, contrast, responsive layouts, locale formatting, help text, product units and no-color-only warnings.
- [ ] **M09-T05** Audit data privacy, untrusted JSON/CSV handling, safe import limits, runtime error recovery and no requirement for login or secret services.
- [ ] **M09-T06** Measure full 76,800-tile viewport/chunk rendering, pan/zoom, route lookups, 40+ firm daily advances, startup generation and save/load on school hardware; optimize without changing economic outcomes.
- [ ] **M09-T07** Verify redistributed visual assets and license manifest, configure Vercel static project, preview/prod builds, locally served sprite paths, routing and deployed gameplay.
- [ ] **M09-T08** Update README setup/build/test/deploy instructions; verify `SPEC.md` ↔ `ROADMAP.md` ↔ `TODO.md` ↔ implemented behavior, and update curriculum caveats.
- [ ] **M09-T09** Verify no critical/high release blockers; tag/version Release 1.0 and record release notes, known limitations within explicit scope, and test evidence.

**M09 exit:** complete, independently reproducible, classroom-ready Release 1.0 published to Vercel; no mandatory criteria deferred.

## 2. Acceptance checklist for the whole release

- [ ] Same seed + same versions + same command history reproduces city, orders, inventory, AI behavior and KPIs.
- [ ] All zones and facilities are implemented on a 320×240 deterministic map, with legal multi-tile lot acquisition, construction, road connectivity and culling.
- [ ] All three product chains produce legitimate goods and retail/wholesale transactions.
- [ ] No transaction bypasses PO/SO, shipment, receiving and settlement rules where applicable.
- [ ] Internal transfers never create external revenue or duplicate goods.
- [ ] Cash, commitments, assets, inventory and ledger are consistent; simplified net profit and ROI are traceable.
- [ ] At least 40 AI counterparties are supplier/customer selectable at each standard completed-day boundary; their entry/exit/expansion/liquidation, orders, ownership and ledgers remain valid.
- [ ] Player can complete the 30-day task through accessible pt-BR interface.
- [ ] Teacher can assign a fixed scenario and evaluate gameplay decisions through a local export.
- [ ] Save/load and import/export preserve pending orders, financial data, PRNG state and replay behavior.
- [ ] CI, security/accessibility checks and Vercel production deployment pass.

## 3. Dependencies, changes and non-goals

**Execution dependencies:** M00→M01; M01→M02/M03; M02+M03→M04; M04→M05→M06; UI foundation from M00 develops in parallel and is fully integrated by M07; pedagogical contracts from M00 and persistence envelope from M01 are completed in M08; M09 is the release-wide gate. See `ROADMAP.md` for the graphical dependency map.

**Change control:** New functionality, revised financial logic or an out-of-scope feature starts as a proposed `SPEC.md` amendment and acceptance-case change. Only after approval add or revise TODO tasks and milestone gates. Never reclassify a required capability as “post-release” merely to clear a milestone.

**Not automatically in Release 1.0:** multiplayer; accounts/backends and teacher cloud dashboards; credit/taxes/inflation; perishability; partial deliveries and disputes; complex vehicle traffic; multi-city geography; external LLM competitors. They remain excluded unless deliberately promoted into the product contract.

**First execution batch recommended:** M00-T01 to M00-T05 (requirements and architectural decisions) followed by M00-T06 to M00-T10 (repository scaffold, CI, design and learning contracts). This is planning/engineering work for the full system, **not** an MVP.
