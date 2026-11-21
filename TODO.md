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
- [ ] **M00-T03** Record architecture decisions for bounded contexts, single state authority, typed command/event contracts, daily phase ordering, stable IDs, PRNG stream partitioning and projected views.
- [ ] **M00-T04** Resolve accounting and logistics ambiguities **before coding**: inventory costing method; asset capitalization and valuation; custody/title during transit; exact recognition/payment boundaries; cash reservation and freight payer; handling pending obligations and insolvency; idempotency keys.
- [ ] **M00-T05** Define product/scenario/save/report schema version policy, deterministic migration/rejection rules and catalog data ownership; define how teacher presets constrain permitted actions.
- [ ] **M00-T06** Establish React + Vite + strict TypeScript project structure matching `SPEC.md` §14, with domain/UI/persistence isolation and path aliases if justified.
- [ ] **M00-T07** Configure package scripts (`dev`, `build`, `typecheck`, `lint`, `test`, `test:e2e`), formatting conventions, GitHub CI, protected review practices and deterministic test seed fixtures.
- [ ] **M00-T08** Define complete UI information architecture, reusable tokens/components, map interaction contract and pt-BR localization approach; include keyboard, focus and screen-reader requirements.
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

- [ ] **M02-T01** Define and validate generation config (dimensions, roads, zone distribution, lot sizes, districts, starting facilities) with version identifier.
- [ ] **M02-T02** Implement seeded, deterministic road graph and shortest-path route/distance calculations; reject unreachable business routes.
- [ ] **M02-T03** Generate all four zones with contiguous districts, legal lots, residential clusters and guaranteed starter-supplier connectivity.
- [ ] **M02-T04** Calculate accessible-demand/footfall indices and nonnegative land prices from documented scenario-configured parameters.
- [ ] **M02-T05** Define lot ownership, purchase eligibility, legal zoning and building placement rules; support the one-building-per-lot constraint.
- [ ] **M02-T06** Implement store/factory/farm/warehouse base entities with ownership, parent lot, status, capacity, operating cost and allowed operations.
- [ ] **M02-T07** Provision player starter store and three AI company networks from validated scenario data without giving them undocumented privileges.
- [ ] **M02-T08** Provide deterministic map/facility read projections for SVG or Canvas rendering and list-based facility navigation.
- [ ] **M02-T09** Test diverse seeds for connectivity, all zone types, road reachability, zoning restrictions, no duplicate lot IDs, and deterministic land values.

**M02 exit:** valid cities with complete structural constraints, stable generation and route-aware ownership/placement.

## M03 — Inventory, commitments and financial integrity

- [ ] **M03-T01** Define integer-unit products, standardized packaging, product eligibility and initial catalogs for fruit, milk, cheese, wood and furniture, with all recipe/source prerequisites.
- [ ] **M03-T02** Implement facility/product stock balances: `onHand`, `reservedOutbound`, `reservedInboundCapacity`, stock value/cost basis and in-transit custody linkage.
- [ ] **M03-T03** Implement atomic stock/capacity reservation, commitment release and available-to-promise calculations; prevent double reservation.
- [ ] **M03-T04** Implement cash/equity balances, earmarked purchase-plus-freight commitments and available-to-spend validation across every payment-capable command.
- [ ] **M03-T05** Implement financial ledger with idempotent posting keys and entries for equity, land/building assets, stock acquisitions, sales, COGS, freight and expenses.
- [ ] **M03-T06** Implement lot/building asset purchases as cash-to-asset changes, never immediate operating losses; verify accounting reconciliation.
- [ ] **M03-T07** Implement daily fixed expenses, obligation recording, insolvency/suspension and player-facing reasons without silently creating negative cash.
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

- [ ] **M05-T01** Implement configurable recipes, facility permissions, input/output quantities, daily batch limits and processing costs.
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

- [ ] **M06-T01** Define three AI companies and configurable deterministic strategies (stock target, price sensitivity, supplier choice, reliability weighting, risk tolerance).
- [ ] **M06-T02** Ensure AI submits the **same typed commands** as the player and passes identical funds/stock/capacity/ownership rules.
- [ ] **M06-T03** Implement AI offer publication, order confirmation/rejection, replenishment and purchasing from either AI or player suppliers.
- [ ] **M06-T04** Implement production planning, stock-aware retail pricing, household market competition and response to shortages.
- [ ] **M06-T05** Implement stable company-ID decision order, commitment tie-breaks, bounded rule evaluation and no knowledge of future random events.
- [ ] **M06-T06** Verify complementary and competing supply networks, including competitor-as-customer, competitor-as-supplier and AI-to-AI purchase chains.
- [ ] **M06-T07** Run 30-day seeded regression campaigns and assert invariants, repeatable events/KPIs, no subsidies unless scenario-documented, and no deadlocks.

**M06 exit:** three AI companies sustainably participate in competitive and cooperative markets under common rules.

## M07 — Complete UI and player interaction

- [ ] **M07-T01** Finalize the app shell, navigation, loading/error/empty states, localized status messaging and complete pt-BR labels/glossary hooks.
- [ ] **M07-T02** Build 2D city map with pan/zoom, roads, districts, lots, facility/owner visuals, selection details and valid construction actions.
- [ ] **M07-T03** Build searchable/list-based facility navigation and ownership/zone/capacity/price/production panels accessible independently of the map.
- [ ] **M07-T04** Build complete Orders experience: supplier comparison, draft/submit, PO/SO mirrored views, transfers, shipment timeline, cancellation and localized rejection explanations.
- [ ] **M07-T05** Build Company Dashboard: revenue, simplified profit, ROI formula breakdown, cash, trends, expense/commitment alerts and facility portfolio.
- [ ] **M07-T06** Build Market and Inventory panels: competing offers, landed costs, ETA, on-hand/committed quantities, recipe availability, replenishment/price actions.
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
- [ ] **M09-T03** Exercise 30-day campaigns with three AI companies and hostile/fault inputs; verify deterministic replay, no duplicate IDs, no invalid state and acceptable execution time.
- [ ] **M09-T04** Audit keyboard/screen-reader experience, contrast, responsive layouts, locale formatting, help text, product units and no-color-only warnings.
- [ ] **M09-T05** Audit data privacy, untrusted JSON/CSV handling, safe import limits, runtime error recovery and no requirement for login or secret services.
- [ ] **M09-T06** Establish and measure performance budgets on documented representative school hardware; optimize simulation and rendering without changing observable rule outcomes.
- [ ] **M09-T07** Configure Vercel static project, connect GitHub, verify preview/prod builds, routing/assets, environment independence and real deployed gameplay.
- [ ] **M09-T08** Update README setup/build/test/deploy instructions; verify `SPEC.md` ↔ `ROADMAP.md` ↔ `TODO.md` ↔ implemented behavior, and update curriculum caveats.
- [ ] **M09-T09** Verify no critical/high release blockers; tag/version Release 1.0 and record release notes, known limitations within explicit scope, and test evidence.

**M09 exit:** complete, independently reproducible, classroom-ready Release 1.0 published to Vercel; no mandatory criteria deferred.

## 2. Acceptance checklist for the whole release

- [ ] Same seed + same versions + same command history reproduces city, orders, inventory, AI behavior and KPIs.
- [ ] All zones and facilities are implemented; legal lot acquisition and construction work.
- [ ] All three product chains produce legitimate goods and retail/wholesale transactions.
- [ ] No transaction bypasses PO/SO, shipment, receiving and settlement rules where applicable.
- [ ] Internal transfers never create external revenue or duplicate goods.
- [ ] Cash, commitments, assets, inventory and ledger are consistent; simplified net profit and ROI are traceable.
- [ ] AI companies buy, sell, produce and compete without bypassing business invariants.
- [ ] Player can complete the 30-day task through accessible pt-BR interface.
- [ ] Teacher can assign a fixed scenario and evaluate gameplay decisions through a local export.
- [ ] Save/load and import/export preserve pending orders, financial data, PRNG state and replay behavior.
- [ ] CI, security/accessibility checks and Vercel production deployment pass.

## 3. Dependencies, changes and non-goals

**Execution dependencies:** M00→M01; M01→M02/M03; M02+M03→M04; M04→M05→M06; UI foundation from M00 develops in parallel and is fully integrated by M07; pedagogical contracts from M00 and persistence envelope from M01 are completed in M08; M09 is the release-wide gate. See `ROADMAP.md` for the graphical dependency map.

**Change control:** New functionality, revised financial logic or an out-of-scope feature starts as a proposed `SPEC.md` amendment and acceptance-case change. Only after approval add or revise TODO tasks and milestone gates. Never reclassify a required capability as “post-release” merely to clear a milestone.

**Not automatically in Release 1.0:** multiplayer; accounts/backends and teacher cloud dashboards; credit/taxes/inflation; perishability; partial deliveries and disputes; complex vehicle traffic; multi-city geography; external LLM competitors. They remain excluded unless deliberately promoted into the product contract.

**First execution batch recommended:** M00-T01 to M00-T05 (requirements and architectural decisions) followed by M00-T06 to M00-T10 (repository scaffold, CI, design and learning contracts). This is planning/engineering work for the full system, **not** an MVP.
