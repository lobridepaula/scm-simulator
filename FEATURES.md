# Supply Chain Simulator — Functional Features

**Document:** FEATURES.md  
**Status:** Committed functional delivery contract; implementation not yet started  
**Target:** One complete, classroom-ready Release 1.0 (not an MVP)  
**Baseline:** 2026-11-21  
**Authoritative rules:** [SPEC.md](./SPEC.md)  
**Implementation sequence:** [ROADMAP.md](./ROADMAP.md)  
**Execution tasks:** [TODO.md](./TODO.md)

## 1. Purpose and interpretation

This document identifies the **game systems and mechanics that must actually work** before Release 1.0 can be considered complete. It is a functional acceptance inventory, not a list of suggested future ideas, a screenshot checklist, or an alternative architecture.

- **MUST / Required:** Release 1.0 cannot ship without the described behavior.
- **SHOULD:** Preferred behavior already identified in SPEC.md, but not a release blocker unless promoted through the product specification.
- **Excluded:** Not part of the committed Release 1.0 scope.
- **Functioning:** A feature accepts a legitimate player or AI action (or an automatic simulation trigger), validates its preconditions, changes authoritative simulation state, produces observable and explainable consequences, survives save/load, and passes deterministic automated verification.
- **No cosmetic substitutes:** Static counters, staged sample companies, fake shipments, arbitrary cash/stock creation, UI-only business logic, or buttons without a domain effect do not satisfy a feature.
- **Authority:** If this inventory and SPEC.md conflict, SPEC.md governs the rule, equation, threshold, processing order, or scope. Resolve the inconsistency before implementation rather than inventing a third rule here.

All mandatory systems are designed from T0 for the full experience. Milestones are integration and verification gates, **not smaller standalone releases**. Feature IDs below are stable traceability keys for issues, tests, and release evidence.

## 2. Release-wide operating baseline

Release 1.0 MUST support the following **together in a normal, playable scenario**, rather than in isolated demonstrations:

1. One procedurally generated **160 × 120 tile city (19,200 tiles)** with four zones, districts, connected roads and variable-sized, buyable lots.
2. One player company with the freedom to operate multiple establishments, choose suppliers and customers, diversify products, and invest in expansion.
3. **Four functional establishments:** store, factory, farm and warehouse, each enforcing its own permitted activities.
4. **At least 60 tradable SKUs across at least 10 connected sectors**, with actual supply sources, eligible buyers and viable economic flows.
5. **50 starting AI companies** and **at least 40 independent, active, trade-accessible AI counterparties at creation and each completed-day boundary**, including **at least 15 supplier and 15 B2B customer options**; roles may overlap.
6. Full procurement, B2B selling, inventory, production, logistics, B2C retail, operating expenses, and business financial reporting.
7. Rule-based competitors that compete and cooperate, with founding, growth, distress, liquidation and closure over time.
8. A **30-simulated-day** standard classroom experience with tutorial, scenario presets, local saves and verifiable teacher-facing results.
9. A complete **pt-BR** player interface, local/offline-capable resources, no mandatory account and no paid AI/backend dependency.

The 160 × 120 map is a capacity constraint: the generator MUST NOT reduce the 50-firm starting population, minimum 40 trade choices, product variety or expansion space to make the layout fit. Infeasible scenarios must be rejected explicitly.

## 3. Simulation systems and required mechanics

### F01 — Scenario initialization, simulation time and rules engine

**References:** SPEC.md §§3, 11, 14, 16; ROADMAP M00–M01.

- **F01.01 — Create scenario:** Start a new company in a validated seeded city, choose a company name and an initial retail product (fruit, cheese or furniture), and receive a legal starter store, recorded opening assets and contributed capital.
- **F01.02 — Immutable scenario identity:** Record seed, generator version, scenario version, catalog version, parameters and ordered command history. Identical inputs must reproduce identical outcomes.
- **F01.03 — Control time:** Pause between days and advance one day at a time. The standard scenario completes after 30 simulated days; available multi-day advance controls must not bypass rules.
- **F01.04 — Execute complete daily cycle:** Use the eleven ordered phases in SPEC.md §11, from player/AI commands through confirmation, transport, production, consumer sales, financial posting, market ecology, metrics and day summary.
- **F01.05 — Validate and apply commands atomically:** Invalid purchases, construction, orders, prices, recipes or transfers explain the error and leave authoritative state unchanged.
- **F01.06 — Reproducible scheduling:** Use seeded independent randomness streams, stable entity IDs and deterministic tie-breaking. AI never acts using knowledge of later outcomes.

**Playable acceptance:** A saved fixed-seed scenario advances through 30 days, produces an auditable daily event sequence, and matches a replay with the same versions and commands, including its AI entry/exit and financial outcomes.

### F02 — Procedural city, geography and spatial economy

**References:** SPEC.md §4; ROADMAP M02, M07.

- **F02.01 — Generate playable geography:** Produce 160 × 120 tiles with major roads, local streets, coherent districts, residential clusters, industrial/logistics locations and agricultural areas.
- **F02.02 — Apply four zones:** Industrial permits factories/warehouses; agricultural permits farms/warehouses; commercial permits stores/warehouses; residential produces consumer demand and permits no company construction.
- **F02.03 — Generate legal lots:** Establish distinct contiguous non-overlapping lot footprints, ownership records, zoning, road-access points and variable lot dimensions.
- **F02.04 — Calculate accessibility:** All active facilities must connect to the road graph; valid shortest-road paths drive freight estimates and lead times. Unreachable routes cannot support an order.
- **F02.05 — Value locations:** Compute nonnegative, data-driven land prices from zoning and accessibility/market attractiveness; explain the determinants to the player.
- **F02.06 — Support map interaction:** Pan, zoom, select lots and establishments, inspect road/district/property detail, and switch zoning, ownership, accessibility, competition and land-value overlays.

**Playable acceptance:** Multiple standard seeds have no overlapping lots, illegal buildings, unreachable active facilities or missing zones; users can find, inspect and purchase an eligible lot without interacting with raw data.

### F03 — Company management, property and establishments

**References:** SPEC.md §§4–5, 10; ROADMAP M02–M03, M07.

- **F03.01 — Manage company portfolio:** The player can view assets, cash, establishments, product participation and active commercial obligations.
- **F03.02 — Acquire land:** Buy a vacant eligible lot only with uncommitted funds; transfer ownership and record a capital asset, not an immediate operating loss.
- **F03.03 — Construct establishments:** Build a store, factory, farm or warehouse only on an owned lot with appropriate zoning, sufficient funds, road access and free capacity for one establishment.
- **F03.04 — Operate multiple facilities:** Each facility maintains a stable identity, owner, capacity, inventory, operating status, fixed cost, location and permitted operations.
- **F03.05 — Expand and diversify:** The starting product limits only the tutorial assortment. The player can add facilities and operate other eligible products/chains, subject to ordinary financing and supply rules.
- **F03.06 — Retire or transfer assets explicitly:** Closing facilities, changing firms or liquidating property must leave traceable title, cash, asset and order history; no implicit deletion or instant free transfer.
- **F03.07 — Charge continuing operation:** Active facilities incur their configured expenses; a player with insufficient uncommitted funds enters the specified insolvency/end-of-run path.

**Playable acceptance:** A player buys a second legal lot, constructs a permitted establishment, incurs its continuing cost and observes the correct cash/asset/profit distinction. Invalid zoning, ownership and cash cases are rejected.

### F04 — Product catalog, sectors and supply-chain topology

**References:** SPEC.md §§3, 6, 8–9; ROADMAP M03, M05–M06.

- **F04.01 — Support a connected catalog:** Provide at least 60 distinct tradable SKU IDs in at least 10 economic sectors with unit, localized label, reference price, source/recipe and buying/selling eligibility.
- **F04.02 — Maintain complete pathways:** Each manufactured SKU has viable upstream input sources and eligible downstream buyers; commodity/source products have legitimate production or extraction rules.
- **F04.03 — Include reference learning chains:** Fruit (farm → store → consumer), cheese (milk producer → cheese factory → store → consumer), and furniture (wood producer → furniture factory → store → consumer) are all executable.
- **F04.04 — Support many-to-many participation:** Multiple firms may offer the same SKU, compete for demand, supply one another, buy intermediate products, or sell through different distribution channels.
- **F04.05 — Use validated data:** Units, sector membership, recipes, prices, demand eligibility and facility permissions are scenario/catalog data, not hardcoded UI examples. Quantity uses nonnegative integer trade units.

**Playable acceptance:** Automated catalog validation proves the 60+/10+ coverage and feasible supply/recipe/buyer graph; all three reference chains can be completed using ordinary game commands.

### F05 — Inventory, reservations and stock control

**References:** SPEC.md §6.3; ROADMAP M03–M05.

- **F05.01 — Separate stock by location:** Track each SKU at each establishment, including physically on-hand stock, reserved outbound stock, inbound reserved space and goods in transit.
- **F05.02 — Make commitments real:** Available-to-promise stock excludes reserved orders. Multiple buyers cannot reserve or sell the same goods twice.
- **F05.03 — Enforce space limits:** New inventory, confirmed inbound shipments and finished production respect destination capacity.
- **F05.04 — Receive and move stock once:** Goods receipt, dispatch, production consumption/output, retail sales and transfer adjust inventory exactly once with defined cost basis.
- **F05.05 — Expose stock decisions:** The player can inspect current/on-order/available stock and inventory value, identify potential shortages and choose whether and when to replenish.
- **F05.06 — Release failed commitments:** Rejected or properly cancelled pre-dispatch orders release held stock and capacity once, without phantom quantities.

**Playable acceptance:** Concurrent procurement/sales attempts cannot create negative inventory or overcapacity; saving/reloading during transit preserves every commitment and physical quantity.

### F06 — Procurement and supplier selection

**References:** SPEC.md §§7–8; ROADMAP M04, M07.

- **F06.01 — Publish actual supplier offers:** Eligible AI suppliers expose company, establishment, SKU, available quantity, unit price and commercial availability.
- **F06.02 — Search and compare suppliers:** Search/filter independent firms and compare unit price, total landed cost, freight, road distance and expected lead time.
- **F06.03 — Choose the supplier:** The player explicitly selects a company and origin facility, a product, a quantity and an eligible destination for a purchase order.
- **F06.04 — Submit feasible orders:** Confirmation requires seller stock, buyer uncommitted cash sufficient for goods plus inbound freight, destination capacity and a valid road route.
- **F06.05 — Preserve economic trade-offs:** Low prices may be offset by higher freight or lead time. Replenishment must reduce spendable cash commitments and subsequently affect stock and financial results.

**Playable acceptance:** A player compares two real AI suppliers, chooses one, submits a PO, receives an unambiguous accepted/rejected outcome and tracks the purchase to receipt and financial settlement.

### F07 — B2B sales, customer selection and quotations

**References:** SPEC.md §7.1–7.3, §9; ROADMAP M04, M06–M07.

- **F07.01 — Publish AI buying needs:** AI customers expose feasible, dated procurement requirements identifying SKU, quantity, target or acceptable price, destination, available budget and validity.
- **F07.02 — Choose a named customer:** The player can search/filter potential buyers, inspect their requests, select an eligible selling facility and prepare a customer-specific sales quotation.
- **F07.03 — Evaluate quotes fairly:** AI accepts or rejects according to documented deterministic rules using matching product, price, quantity, cash, seller stock, delivery route and destination space.
- **F07.04 — Convert acceptance to one order:** An accepted quote produces one authoritative commercial transaction, visible as an AI buyer's PO and player's seller-side SO.
- **F07.05 — Enforce actual fulfillment:** A sale is not revenue just because a customer requested or accepted a quote. Goods must dispatch, arrive, be received and be settled.
- **F07.06 — Expose rejection reasons:** A rejected/expired quote never moves stock or money and gives a comprehensible reason; buyers cannot acquire free goods or invisible funding.

**Playable acceptance:** The player finds a named AI buying requirement, submits a quote and completes an accepted sale through ordinary shipment/payment rules; rejection is demonstrably side-effect free.

### F08 — Commercial orders, transportation and internal transfers

**References:** SPEC.md §7; ROADMAP M04.

- **F08.01 — Keep one canonical B2B order:** PO and SO are buyer/seller projections of the same underlying trade, not separate settlements.
- **F08.02 — Enforce lifecycle:** Support draft, submitted, confirmed, rejected, cancelled, dispatched, in transit and delivered statuses, with only valid transitions.
- **F08.03 — Reserve on confirmation:** Once confirmed, earmark seller available stock, buyer cash for product plus freight, and destination receiving capacity.
- **F08.04 — Simulate physical delivery:** Create traceable shipments, dispatch goods from their origin, calculate travel using road distance and scenario-defined logistics speed/cost, and deliver no earlier than the following game day.
- **F08.05 — Receive and settle exactly once:** On arrival, update receiving stock and inventory landed cost; transfer goods payment to the seller; charge inbound freight to the buyer; recognize seller revenue and COGS exactly once.
- **F08.06 — Support cancellation rules:** Reject invalid offers; cancel eligible pre-dispatch orders and undo reservations. Do not allow unsupported post-dispatch cancellation or partial shipment.
- **F08.07 — Transfer within a firm:** Stock transfers between company-owned facilities use transfer orders and physical routing. They preserve company-wide goods, carry inventory cost and never generate external revenue or consolidated COGS.
- **F08.08 — Expose full history:** Inspect PO/SO roles, shipment state, origin, destination, promised and actual delivery day, logistics cost and settlement outcome.

**Playable acceptance:** Purchase, seller-originated sale, AI-to-AI trade and internal transfer each conserve stock and money under repeat commands, contention and save/load. Duplicate receipts or settlement attempts are idempotent.

### F09 — Farming, manufacturing and warehousing

**References:** SPEC.md §§5–6, 11; ROADMAP M05.

- **F09.01 — Extract primary resources:** Farms produce legitimate catalog resources subject to permitted type, productivity/capacity and configured harvesting or operating costs.
- **F09.02 — Manufacture from recipes:** Factories select permitted recipes, require exact input amounts, processing budget and free output capacity, and convert inputs into finished units only through the scheduled production phase.
- **F09.03 — Preserve material cost:** Finished-goods inventory cost includes consumed input value and paid processing cost; value becomes COGS only upon an external sale.
- **F09.04 — Control throughput:** Recipes and facilities have daily production limits; production cannot create free outputs, negative inputs or finished goods beyond capacity.
- **F09.05 — Respect order timing:** Newly manufactured or harvested output cannot retroactively fulfill an earlier same-day dispatch phase.
- **F09.06 — Store and redistribute:** Warehouses receive, hold and dispatch products and support transfers, but neither manufacture goods nor sell directly to households in Release 1.0.

**Playable acceptance:** All three tutorial chains operate with real inputs, processing, shipment and sale. Tests prove conservation, financial cost carry-over and facility throughput constraints.

### F10 — Residential demand, retail and competitive pricing

**References:** SPEC.md §§4, 8, 10; ROADMAP M05–M06.

- **F10.01 — Generate bounded household demand:** Residential districts have seeded population/activity, product preferences and daily purchase quotas.
- **F10.02 — Serve a geographic market:** A store's eligible demand reflects district accessibility and reachable consumers rather than an unlimited global buyer pool.
- **F10.03 — Compete for customers:** Stores offering the same products share demand according to deterministic price, attractiveness, availability and competition logic.
- **F10.04 — Let the player set prices:** Permit valid retail price adjustments and display their relation to expected customer demand, margin and operating performance.
- **F10.05 — Sell only real goods:** B2C sales draw from unreserved on-hand inventory, do not exceed household demand or stock, and issue traceable sales receipts.
- **F10.06 — Record missed opportunities:** Stockouts or unattractive offers can result in fewer sales; expose unfulfilled demand, stockout days and relevant sales indicators.

**Playable acceptance:** Two competing retail stores share finite local demand; changing price or supply can affect their sales, while no store sells nonexistent goods and the resulting revenue/COGS reconcile.

### F11 — Autonomous company behavior and market ecology

**References:** SPEC.md §§5, 9–11; ROADMAP M02, M06.

- **F11.01 — Populate the city:** Generate 50 independent AI firms initially across a broad set of sectors, with feasible road-connected establishments and real offers or purchasing requests.
- **F11.02 — Maintain meaningful market choice:** At scenario creation and every completed simulated day, provide at least 40 distinct active, independent, trade-accessible AI companies, including at least 15 supplier options and 15 B2B customer options. Decorative, unreachable, insolvent or closed firms do not count.
- **F11.03 — Let companies trade with each other:** AI may purchase from the player or other AI firms, sell to either, compete for residential demand, and use complementary or competing production networks.
- **F11.04 — Make operational decisions:** AI uses configurable, deterministic policies for publishing offers/requests, supplier selection, stock replenishment, production, retail price and resource allocation.
- **F11.05 — Apply the player's rules to AI:** Each company uses the same typed commands and stock, cash, property, routing, production, order and capacity validation. No privileged free inventory or arbitrary settlement.
- **F11.06 — Found new companies:** Score actual market opportunities and open funded, legally located firms through a scenario-defined finite external-investor equity pool; document contributed capital rather than treating it as operating revenue.
- **F11.07 — Allow growth and retreat:** AI may invest in lots/facilities, diversify, scale production, suspend operations, dispose of assets or contract after sustained poor performance.
- **F11.08 — Resolve distress and closure:** Maintain explicit lifecycle states, insolvency handling, asset/liability disposition, pending-order and shipment resolution, historical records and dated closure events.
- **F11.09 — Maintain the market floor honestly:** A validated, deterministic funded entrant pipeline may replace failed firms. If legal space or funding is insufficient, reject the scenario as infeasible rather than secretly creating counterparties, money or inventory.
- **F11.10 — Keep decisions bounded and replayable:** Stable company ordering, appropriate planning cadences and indexed geographic/sector data support 30-day play without changing simulation outcomes.

**Playable acceptance:** In repeatable 30-day standard campaigns, companies transact, compete, enter and exit; at every completed-day boundary 40+ legitimate trading counterparts remain discoverable, and all financial/asset/order history survives closure and save/load.

### F12 — Finance, operating performance and business failure

**References:** SPEC.md §10; ROADMAP M03–M06.

- **F12.01 — Track business cash separately:** Maintain contributed equity, spendable cash, funds committed to purchases and explicit cash movements in integer BRL cents.
- **F12.02 — Record investment as assets:** Purchasing land, buildings or inventory affects cash and owned value, not immediate operating profit or COGS.
- **F12.03 — Recognize genuine external sales:** Revenue and associated COGS arise only from completed B2B deliveries and actual household sales, never accepted-only orders, internal transfers or equity contributions.
- **F12.04 — Recognize operating expenses:** Facility fixed costs and defined business expenses reduce profit; manufacturing cost is first capitalized into inventory. Freight treatment follows SPEC.md §10.
- **F12.05 — Display required KPIs:** Revenue (faturamento), simplified simulated net profit (lucro líquido), ROI and cash, with period, derivation and explanatory definitions. Allow negative profit and ROI.
- **F12.06 — Display operational KPIs:** Orders by state, on-time delivery, stockout/unmet demand, facility utilization, on-hand/reserved inventory and valuation.
- **F12.07 — Explain insolvency:** Unpayable mandatory costs cause a recorded obligation, suspension and explicit failure/end-of-run explanation for the player; never silently make cash negative or waive the cost.
- **F12.08 — Reconcile ledgers:** Every monetary event has a stable traceable origin and is posted once; reports derive from authoritative financial and inventory events rather than independently calculated UI totals.

**Playable acceptance:** Buying property reduces cash but not immediate profit; purchasing goods does not create COGS until sale; an external sale changes revenue/profit correctly; two runs with the same events have identical ledgers and KPI results.

### F13 — Player interface, information and decision support

**References:** SPEC.md §§4, 12, 15; ROADMAP M07.

- **F13.01 — City workspace:** Usable map, overlays and property/facility selection linked to the real geographic simulation.
- **F13.02 — Company dashboard:** Current cash, revenue, simplified profit, ROI, trends, establishments, outstanding orders and actionable operating alerts.
- **F13.03 — Market directory:** Search/filter 40+ accessible firms by supplier/customer role, sector, product, district, offers and buying requirements; reveal lifecycle changes.
- **F13.04 — Procurement and sales workspace:** Choose named suppliers and named AI buyers, compare options, submit POs/quotes, see mirrored PO/SO, status and exceptions.
- **F13.05 — Facility workspace:** View stock, available/reserved quantities, capacity, recipes, production options, product offers, retail pricing and internal transfers.
- **F13.06 — Operational transparency:** Day summaries, shipment progress, event history, errors and glossary explain how the user's choices led to outcomes.
- **F13.07 — Accessible pt-BR gameplay:** Brazilian currency/units, keyboard and pointer support, visible focus, text/icon alternatives to color, readable contrast, map-independent company/facility navigation and no audio requirement.

**Playable acceptance:** A learner can complete every mandatory game action without developer tools, direct JSON editing or a hidden debug interface; visible values match authoritative domain state.

### F14 — Learning objectives, teacher scenarios and classroom assessment

**References:** SPEC.md §13; ROADMAP M08.

- **F14.01 — Guide initial learning:** Provide an optional walkthrough of first product, supplier comparison, PO, shipment receipt, retail sale and financial interpretation.
- **F14.02 — Make objectives observable:** Record completion of lessons/missions using actual player actions and simulation events, not a decorative completion toggle.
- **F14.03 — Support teacher presets:** Configure fixed seed, scenario/curriculum objective, starting equity, facilities, enabled products/actions, session length and assessment criteria.
- **F14.04 — Complete 30-day sessions:** Present an end-of-session summary explaining financial outcomes alongside purchasing, inventory, service and investment decisions.
- **F14.05 — Capture reasoning:** Allow optional learner reflection and preserve a chronological decision/evidence history for assessment.
- **F14.06 — Report educational evidence:** Associate observable results with the suggested rubric and BNCC-aligned activities, without claiming formal curriculum certification.

**Playable acceptance:** A teacher can assign the same fixed-seed preset to separate students, each can complete the guided scenario, and both can export valid, comparable evidence without registration.

### F15 — Persistence, replay and export

**References:** SPEC.md §§11, 13–15; ROADMAP M01, M08.

- **F15.01 — Save locally:** Save active games to IndexedDB with current day, scenario/catalog versions, seeds/PRNG state, map, companies, lots, offers/requests, inventories, commitments, orders, shipments, AI lifecycle and finance.
- **F15.02 — Resume accurately:** Reload mid-order or mid-shipment and continue without lost reservations, duplicated receipts, altered AI decisions or changed accounts.
- **F15.03 — Import/export safely:** Support versioned JSON save export/import with schema and size validation; migrate compatible versions explicitly or reject incompatible ones with an intelligible message.
- **F15.04 — Export learning outcomes:** Generate local CSV/JSON reports containing scenario identity, seed, versions, day, business KPIs, order activity, objectives and timestamped decision history.
- **F15.05 — Reproduce outcomes:** Fixed seed, versions and ordered actions recreate equivalent map, events, company lifecycle, inventory and financial results, including after save/load.

**Playable acceptance:** A 30-day scenario restarted from a mid-shipment save reaches the same final game state and teacher report as an uninterrupted deterministic run.

### F16 — Game reliability and release-wide quality gates

**References:** SPEC.md §§14–16; ROADMAP M00–M09.

- **F16.01 — Preserve modularity:** A framework-independent TypeScript simulation is authoritative; React/Canvas only displays projections and sends typed commands.
- **F16.02 — Make standard city usable:** Render the full world with viewport culling, cached chunks and indexed lot/facility interaction rather than one DOM component for every tile.
- **F16.03 — Meet realistic performance:** Measure generation, daily advances, navigation, route queries, save/load and memory on representative classroom hardware at 160 × 120, 50 starting firms and the full product catalog.
- **F16.04 — Run without paid dependencies:** Use locally served, license-verified visual assets and rule-based AI; publish a static browser application through GitHub/Vercel without required credentials or a paid simulation API.
- **F16.05 — Protect learner privacy:** Do not require student names, accounts or cloud storage; reject malformed or oversized imported files and preserve accessibility requirements.
- **F16.06 — Validate continuously:** Run typechecking, linting, automated domain and scenario tests, build verification, browser critical-path tests and release acceptance checks before declaring completion.

**Playable acceptance:** A production-deployed game passes deterministic, functional, accessibility, import/export and full-load browser verification without hidden backend services.

## 4. Required cross-system player journeys

The following tests are **end-to-end gameplay contracts**. Passing isolated unit tests is not sufficient.

### Journey J01 — Procurement to household sale

1. Start a seeded standard city and select cheese as the initial product.
2. Find at least two independent cheese suppliers and compare unit cost, freight and expected delivery.
3. Select one supplier, submit a PO and see the corresponding seller SO.
4. Advance through dispatch, transit, receiving and exactly-once settlement.
5. Change retail price if desired; sell available cheese to bounded residential demand.
6. Inspect inventory depletion, revenue, COGS, cash, net profit and ROI.
7. Save, reload and confirm that order history, inventory and metrics remain intact.

### Journey J02 — Vertical integration and diversification

1. Buy a permitted connected lot using available cash.
2. Construct a farm, factory, store or warehouse appropriate to its zone.
3. Source ingredients, produce through a valid recipe and/or transfer goods between company facilities.
4. Sell the resulting goods externally; verify throughput, lead time, operating costs and inventory valuation.
5. Confirm that an internal transfer did not create sales revenue and asset acquisition did not immediately reduce profit.

### Journey J03 — Player-selected AI customer

1. Find a named active AI firm with a feasible published buying request.
2. Select a selling facility with available matching stock; submit a quote.
3. Observe deterministic acceptance/rejection without hidden funds or inventory.
4. If accepted, fulfill one canonical PO/SO through real shipment and settlement.
5. Verify the player receives revenue only on completed external delivery and the AI's stock/funds reconcile.

### Journey J04 — Evolving and competitive AI economy

1. Run the default 160 × 120 city with 50 initially generated AI firms and the complete catalog.
2. Verify 40+ legitimate trade-accessible AI firms (including 15 suppliers and 15 buyers) at creation and after every completed day.
3. Observe AI-to-AI trade, competitors selling to the same market, replenishment, pricing and at least one viable founded or closed firm in dedicated lifecycle test fixtures.
4. Verify asset and pending-order resolution when a company closes; preserve its history.
5. Replay the run to confirm identical firm sequence, transactions and results.

### Journey J05 — Classroom completion and evidence

1. Play the 30-day teacher preset with fixed seed and optional instruction.
2. Complete the purchase/sale and decision-analysis tasks using real mechanics.
3. Reach the final report; explain cash versus profit versus ROI and relevant supply decisions.
4. Export a teacher-readable CSV/JSON report and optionally include a reflection.
5. Compare two independent attempts from the same preset while preserving differences caused by the students' decisions.

## 5. Release acceptance matrix

| Gate | Required evidence | Key features | Milestone |
| --- | --- | --- | --- |
| Deterministic core | Same seed/versions/actions produce identical events, state and KPIs | F01, F15 | M01, M09 |
| Valid economic city | 19,200 tiles, four zones, reachable legal lots/facilities, available expansion sites | F02, F03 | M02, M09 |
| Real product networks | 60+ SKUs, 10+ sectors, fruit/cheese/furniture chains validated end to end | F04, F09 | M03, M05, M09 |
| Resource and cash integrity | No negative/duplicated stock, overcommitted cash, overflowed capacity or double postings | F05, F08, F12 | M03, M04, M09 |
| Two-way B2B trading | Player buys from selected AI supplier and sells to selected AI customer via PO/SO and shipments | F06–F08 | M04, M06, M09 |
| Real production and markets | Input conversion, finite household demand, retail competition and actual sales | F09, F10 | M05, M09 |
| Dynamic AI company population | 50 starting firms; 40+ active counterparts at day boundaries; 15 buyer/15 supplier choices; traceable entry/exit | F11 | M06, M09 |
| Complete playable interface | Every major command usable in pt-BR without developer intervention | F13 | M07, M09 |
| Assessable classroom experience | 30-day lessons, same-seed presets, learning evidence, report | F14 | M08, M09 |
| Durable and publishable application | Accurate local saves, replay, safe imports, exports, accessibility, measured performance and deployed build | F15, F16 | M08, M09 |

**Definition of Done for a feature:** its engine behavior is implemented; reachable through the prescribed UI or automatic scheduler; observable through actual state/events; covered by positive, negative, conservation and save/load tests as applicable; and its documentation, localization and relevant milestone evidence are current. A feature is not Done solely because its interface exists.

**Release gate:** All mandatory features F01–F16 and journeys J01–J05 work together, the invariants in [SPEC.md §16](./SPEC.md#16-verification-and-acceptance-criteria) pass, and the complete Release 1.0 is usable on the intended classroom devices. No mandatory feature is moved to “post-release” merely to clear a milestone.

## 6. Excluded systems and controlled extensions

The following are **not promised for Release 1.0**: multiplayer; student accounts/cloud classrooms; realistic taxes, banking, debt, credit and inflation; perishable or quality-graded goods; partial shipments, returns and disputes; auction markets and recurring contractual negotiation; detailed vehicle fleets, traffic or accidents; multi-city geography; and generative AI/paid API inference.

These exclusions are boundaries, not hidden missing tasks. Adding one requires an approved change to SPEC.md, the appropriate feature contract and acceptance tests, ROADMAP.md and TODO.md.

## 7. Documentation and traceability rules

- **SPEC.md** defines authoritative economic/spatial rules, formulas, thresholds, invariants and processing order.
- **FEATURES.md** defines the game systems and what a functioning, releasable player experience must demonstrate.
- **ROADMAP.md** defines milestone sequencing and cross-system gates.
- **TODO.md** defines executable engineering tasks and issue IDs.
- **README.md** summarizes the product without redefining its contractual scope.
- Each implementation issue or pull request SHOULD reference feature IDs (for example, F08.05), the matching SPEC.md section, the TODO task ID, tests and a verifiable user-facing outcome.
- If a feature is modified, update the authoritative rule and its verification evidence in the same change. Do not add competing copies of financial equations or engine scheduling definitions to this inventory.

---

**Document owner:** Supply Chain Simulator project.  
**Status note:** This describes the required finished game; the repository currently contains documentation only. No item above should be interpreted as implemented without code and passing evidence.
