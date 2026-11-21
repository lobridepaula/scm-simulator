# Supply Chain Simulator — Product and Technical Specification

**Document:** `SPEC.md`  
**Version:** 0.2 — Expanded world and market baseline  
**Status:** Draft for implementation  
**Date:** 2026-11-21  
**Target platform:** Web browser (desktop-first; responsive)  
**Repository:** GitHub (to be created)  
**Deployment:** Vercel  
**Primary implementation language:** TypeScript  
**Learners:** Brazilian students of the *Assistente Administrativo de Projetos* course  
**Player language:** Brazilian Portuguese (`pt-BR`); internal source code, identifiers, and technical documentation: English

> **Document intent:** This document defines the complete Release 1.0 contract, implemented incrementally from T0 without a separately scoped MVP. Statements marked **MUST**, **MUST NOT**, and **SHOULD** govern requirements. Scenario balance is configurable; excluded capabilities require explicit change control.

---

## 1. Vision

Build a lightweight, browser-based supply-chain and business-management simulation inspired by the accessible economic feedback loops of *Capitalism Lab*, without reproducing its assets, interface, proprietary content, or full complexity.

The player is the director of a company operating in a procedurally generated city. The company must source goods, fulfill orders, manage stock, produce or distribute products, compete or cooperate with AI companies, and invest in additional establishments.

**Primary learning objective:** understand how operational decisions propagate through purchase orders, sales orders, deliveries, inventory, production, revenue, costs, and profit.

**Primary gameplay loop:**

1. Inspect demand, inventory, suppliers, and financial position.
2. Select a product and determine purchasing or production needs.
3. Issue or accept commercial orders.
4. Wait for suppliers to confirm, dispatch, and deliver goods.
5. Receive inventory, produce if applicable, and sell products.
6. Review orders, service levels, revenue, profit, cash, and ROI.
7. Improve prices, suppliers, capacity, locations, and product mix.

The game MUST make the consequences of decisions visible. It MUST NOT require a prior understanding of business software to be playable.

## 2. Educational and Product Principles

1. **Orders first.** Trade between two establishments MUST be represented by a traceable commercial order. A single transaction exposes a purchase-order view for its buyer and a sales-order view for its seller.
2. **Learning through consequence.** Excess purchasing ties up cash; insufficient purchasing causes stockouts; slow or expensive suppliers affect margins and service.
3. **Low entry barrier.** A student MUST be able to perform a meaningful first purchase within the introductory activity.
4. **Economic consistency.** Financial and inventory updates MUST be derived from actual simulated events, not cosmetic dashboard values.
5. **Deterministic replay.** A seed, scenario version, and ordered sequence of commands MUST reproduce the same simulated outcome.
6. **Modular design.** Rules for geography, purchasing, inventory, manufacturing, logistics, finance, demand, and AI MUST be separable and testable without a browser.
7. **Teacher-oriented.** A teacher SHOULD be able to assign identical scenarios and evaluate both results and decisions.
8. **No paid services required.** The initial game MUST operate without accounts, an API key, or a backend service.
9. **Accessible interaction.** Gameplay MUST NOT rely solely on color, animation, hover, or fine pointer control.

## 3. Target Experience and Scope

### 3.1 Initial player experience

- The player creates or names a company and chooses an initial commercial product: **fruit**, **cheese**, or **furniture**.
- The tutorial scenario grants a small starter retail establishment on a valid commercial lot, with the land/building reflected as opening assets. This prevents construction from blocking the first lesson.
- The player receives an initial equity contribution, an operating cash balance, and access to supplier listings.
- The city contains **at least 40 simultaneously trade-accessible AI companies**, spanning complementary and competing sectors, with company formation and closure throughout play.
- The player must issue at least one purchase order, receive the goods, sell them to consumers, and inspect the financial results.
- Later in the same game, the player may buy an eligible lot, build another establishment, and buy from or sell to AI companies.

The product choice determines the initial retail assortment, not a permanent company restriction. Vertical integration and diversification remain possible.

### 3.2 Release 1.0 — in scope

- Deterministic **320 × 240 tile (76,800 tile)** city, four zones, road hierarchy, districts and variable-sized lots.
- Buyable lots, zoning restrictions, and location-based land prices.
- Four establishment types: `store`, `factory`, `farm`, `warehouse`.
- Broad multi-category product catalog and configurable manufacturing recipes; fruit, cheese and furniture are educational starter chains, not the full catalog.
- Distinct inventory per establishment.
- Business-to-business (B2B) purchase/sales orders, delivery lifecycle, and receiving.
- Basic consumer (B2C) retail sales and demand.
- At least 40 active, trade-accessible AI company counterparties at standard scenario day boundaries, with endogenous market entry, expansion, distress, liquidation and closure.
- Cash accounting, recognized revenue, cost of goods sold, operating expenses, profit, ROI, and supporting metrics.
- 30-day scenario, pause/advance-day control, basic tutorial, teacher scenario presets, local saving, and results export.
- Viewport-culled 2D city map with selectable lots, establishments, property overlays and a searchable market-company directory.

### 3.3 Explicitly excluded from Release 1.0

- Multiplayer and synchronous student-to-student trading.
- Realistic taxation, legal compliance, labor contracts, inflation, banking, debt, and credit terms.
- Complex transportation fleets, traffic simulation, road accidents, or vehicle animation as game rules.
- Perishable goods, quality grades, spoilage, partial deliveries, returns, and disputes.
- Auctions, complicated negotiation, speculative markets, and dynamic supply-chain contracts.
- Generative AI or external LLM calls for competitors.
- Account creation, user profiling, teacher dashboards hosted on servers, or collection of student identity data.
- Multiple cities and real-world geography.

These are extension candidates, not implicit implementation tasks.

## 4. World and Map

### 4.1 Procedural generation and spatial hierarchy

The city is generated from a seed and versioned world generation configuration.

- **Default city size MUST be 320 × 240 tiles (76,800 total)**. These are configurable dimensions, but the standard Release 1.0 test fixtures MUST use this default.
- A tile is a terrain/rendering cell. A lot is a permanent, non-overlapping contiguous footprint of tiles. Districts group lots and residential demand with distinct geographic/economic profiles.
- Generate major arterial roads, collector/local streets, downtown and secondary centers, industrial/logistics areas, agricultural edges and residential clusters. All four zoning types MUST be represented.
- Generate road-connected rectangular or otherwise validated lots with sizes suitable for stores, factories, farms and warehouses; zoning and ownership are evaluated at the lot level.
- All operational establishments MUST have road access. Shortest valid road-path distance determines transport time and freight costs.
- Seeded company allocation MUST yield a player starter site and at least 40 distinct AI counterparties with viable supplier and/or customer opportunities; an inaccessible decorative firm does not count.
- Seed, generator/scenario/catalog versions and ordered command history MUST reproduce the complete map, firm founding sequence, closures, orders and financial results. PRNG streams for world, demand, AI and market entry MUST be partitioned.

**Generation order:** macro roads/districts → secondary roads/blocks → lots and zoning → population/land value → firms and facilities → route/market-coverage validation.

**Map controls:** pan, zoom, select lot/facility, switch ownership/zone/market/transport overlays and navigate through non-map company/facility listings.

### 4.2 Zoning

| Zone | Map color | Permitted construction | Economic purpose |
|---|---|---|---|
| Industrial | Red | Factory, warehouse | Manufacturing and distribution |
| Agricultural | Yellow | Farm, warehouse | Extraction, cultivation, animal products |
| Residential | Green | None | Household demand and movement generation |
| Commercial | Blue | Store, warehouse | Retail sales and distribution |

- A building MUST be rejected if its type is not permitted in the selected zone.
- No player or AI company may construct in a residential zone in the first release.
- A lot MUST be owned by the constructing company before construction begins.
- A lot initially hosts at most one establishment.

### 4.3 Lots, accessibility, and land value

Each lot has an immutable ID, contiguous tile footprint, zoning, connected road-access node, purchase price, owner and optional establishment. Footprints MUST NOT overlap roads or other lots.

Land value SHOULD depend on a transparent simplified movement/accessibility index. For example:

`landPrice = baseZonePrice * (1 + accessibilityIndex * locationPremiumFactor)`

where `accessibilityIndex` is a normalized, seeded approximation of nearby household activity and road connectivity. The price formula MUST be documented and nonnegative.

- Higher footfall generally increases commercial attractiveness and lot prices.
- Road distance MUST influence lead time and inbound transport costs.
- No visual pedestrian agents are required: activity may be represented by deterministic aggregated demand values.
- Purchase of a lot is a capital investment, NOT an immediate operating expense.

### 4.4 Development, ownership and retirement of facilities

- A lot may have one operating establishment at a time; the company and lot remain separate entities, and companies may own many lots.
- Construction validates ownership, zone, road access, footprint, funds and active company status.
- Closing a facility or company MUST NOT silently delete the lot, free property ownership, erase order history, or invent a sale. Redevelopment and asset transfer are explicit recorded transactions.
- Market attractiveness and lot pricing SHOULD use cached district/road aggregates rather than scan all tiles daily.

### 4.5 Rendering and asset management

- Prefer **Canvas 2D with viewport culling, reusable cached map chunks, and spatial indices for lot/facility hit-testing**; avoid a React/DOM node per tile.
- Keep the economic world model independent of textures, UI coordinates and sprites.
- Use a versioned asset manifest with locally served terrain, road, lot and facility sprites, a fallback for missing art, and recorded provenance/license.
- **Kenney Tiny City** is a candidate visual baseline, pending verification of the actual pack, CC0 terms, redistribution rights and compatibility before bundling; other packs or original vector work may fill gaps.
- No network image service or visual asset dependency is required while playing. Overlay labels/icons/patterns MUST supplement colors.

## 5. Economic Actors and Establishments

### 5.1 Companies

A company has an ID, owner type (`player` or `ai`), name, cash balance, equity capital contributed, ownership of lots and establishments, product offers, orders, financial ledger, and decision policy.

- Any company MAY trade with any other company if the deal is feasible.
- Competitors MAY also be suppliers or customers.
- Two companies MAY use the same product and compete for the same residential market.
- Within-company goods movements MUST be recorded as **stock transfer orders**, not as external sales or company revenue.
- Each company has a stable ID, industry, lifecycle state, founded day, facilities, offers/requests and ledger. One company can compete with and supply another.
- **Trade-accessible** means discoverable in the company directory, reachable via the road network, operational and having at least one feasible B2B supplier offer and/or buyer purchasing opportunity under catalog rules.
- Standard classroom scenarios MUST start with, and finish each completed simulated day with, **at least 40 distinct AI counterparties**, excluding the player. The guarantee refers to available counterparty choice, not mandatory transactions with 40 firms.

### 5.2 Establishment types

**Store (`store`)**
- Receives finished goods and may sell configured goods to the consumer market.
- Has stock capacity, retail prices, local demand exposure, and daily operating expense.
- May also fulfill valid B2B orders if offering merchandise for resale is enabled by scenario data.

**Factory (`factory`)**
- Consumes specified input products and manufactures recipe outputs.
- Has input/output inventory, recipe permissions, production capacity per day, and manufacturing costs.
- Can sell to stores, warehouses, or other factories through B2B orders.

**Farm (`farm`)**
- Extracts or harvests primary products using source-specific production rules.
- Can produce agricultural goods or raw resources as specified in a configurable resource catalog.
- Can sell directly to stores, warehouses, or factories.

**Warehouse (`warehouse`)**
- Receives, holds, and dispatches products subject to stock capacity.
- Does not transform goods or sell to households in the initial release.
- Allows inter-establishment transfers and consolidation; its benefit is spatial and stock-management flexibility.

Each establishment MUST have a unique ID, company ownership, parent lot, current inventory, capacity, operational status, daily fixed cost, and permitted product operations.

## 6. Products, Recipes, and Stock

### 6.1 Products

Every product requires a unique ID, localized display name, unit, category, reference price, purchase/sale eligibility, and optional recipe.

The broader Release 1.0 catalog MUST span multiple connected product families (including agricultural goods, raw materials, industrial inputs and finished consumer goods) and support realistic supplier/customer selection across the active companies. Product and recipe graphs MUST be validated for sources and viable buyers.

Recommended starter chains:

- **Fruit:** farm → store → consumer.
- **Cheese:** dairy farm produces milk → cheese factory consumes milk → store → consumer.
- **Furniture:** forestry resource producer supplies wood → furniture factory consumes wood → store → consumer.

All product quantities in the initial release MUST be nonnegative integers representing standardized trade units (e.g., one packaged liter, one fruit box, one furniture unit). Display units MUST identify the packaging convention. Fractional quantities can be introduced later.

### 6.2 Production and extraction

A production recipe defines input quantities, output quantity, processing cost, authorized establishment type, and daily throughput.

Example data representation:

```json
{
  "id": "recipe.cheese",
  "facilityType": "factory",
  "inputs": [{ "productId": "milk", "quantity": 4 }],
  "output": { "productId": "cheese", "quantity": 1 },
  "processingCostCents": 300,
  "batchesPerDay": 10
}
```

- Production MUST NOT begin without available inputs, enough output capacity, and enough cash for its processing cost.
- Production consumes inputs and adds completed outputs exactly once.
- A minimal one-day production cycle is sufficient; new outputs become tradable in the next day’s order cycle.
- The cost basis of outputs MUST include consumed material costs and processing costs. It transfers to cost of goods sold only when finished goods are sold externally.
- Resource extraction by farms uses configured output quantities and operating/harvesting costs; it MUST NOT create goods for free unless explicitly defined by a tutorial scenario.

### 6.3 Inventory

Stock is tracked per `{facilityId, productId}` and MUST distinguish:

- `onHand`: physically present stock.
- `reservedOutbound`: on-hand stock committed to accepted outbound orders.
- `reservedInboundCapacity`: space committed to confirmed inbound deliveries.
- `inTransit`: goods on an active shipment, represented with custody and ownership defined by shipment/order rules.

`availableToPromise = onHand - reservedOutbound`.

Inventory invariants:

- No negative `onHand`, reserved values, cash, or shipment quantity in initial scenarios.
- Reservations MUST NOT exceed available stock or incoming capacity.
- Total physical stock plus committed inbound reservations MUST NOT exceed establishment capacity at receipt.
- Receiving goods MUST move quantity from transit into the destination stock exactly once.
- Cancelling a pre-dispatch order MUST release all associated reservations exactly once.
- A warehouse transfer MUST NOT create or destroy stock or company-wide profit.

## 7. Purchase Orders, Sales Orders, and Logistics

### 7.1 Commercial transaction model

**B2B trade between different companies MUST be documented by one authoritative commercial order record** with mirrored buyer/seller views:

- Purchase Order (PO): what the buying establishment requests.
- Sales Order (SO): the same confirmed transaction from the seller’s perspective.
- Shipment: physical transfer of ordered goods from seller to buyer.
- Goods receipt: destination acceptance and inventory registration.
- Settlement: recognition of cash payable/receivable according to the simplified payment rule.

The engine MUST NOT create two independent financial transactions merely because users see both PO and SO views.

**B2C retail** uses automatically generated aggregated consumer orders/sales receipts. Household purchases MUST consume store inventory and generate revenue but do not require an establishment-to-establishment PO.

**Internal transfers** between establishments belonging to one company use a transfer order, inventory movement, and optional logistics cost; they NEVER count as revenue or COGS for consolidated company performance.

### 7.2 Order schema

```ts
type OrderStatus =
  | 'draft'
  | 'submitted'
  | 'confirmed'
  | 'rejected'
  | 'cancelled'
  | 'dispatched'
  | 'in_transit'
  | 'delivered';

interface CommercialOrder {
  id: string;
  buyerCompanyId: string;
  buyerFacilityId: string;
  sellerCompanyId: string;
  sellerFacilityId: string;
  productId: string;
  quantity: number;
  unitPriceCents: number;
  inboundFreightCents: number;
  submittedDay: number;
  promisedDeliveryDay: number | null;
  actualDeliveryDay: number | null;
  status: OrderStatus;
}
```

The implementation MAY use a normalized multi-line schema later. For the first release, one product per order keeps explanations and inventory reservations straightforward.

### 7.3 Order lifecycle

`draft → submitted → confirmed → dispatched → in_transit → delivered`

Alternative terminal paths:

- `submitted → rejected`.
- `draft/submitted/confirmed → cancelled` if dispatch has not occurred.

Required behavior:

1. **Draft:** player chooses supplier, destination, product, quantity, and quoted price.
2. **Submit:** validate destination, supplier’s offer, buyer cash availability, and requested quantity. Submission creates an auditable order number.
3. **Confirm:** supplier accepts only if feasible. Reserve seller stock and destination capacity; reserve/earmark buyer funds for the total purchase value **and inbound freight**, so subsequent orders cannot overcommit cash.
4. **Dispatch:** release stock from the seller’s on-hand location into shipment custody and mark dispatched/in transit.
5. **Transport:** shipping time depends on road-network distance and configured logistics speed. Minimum lead time is one simulation day.
6. **Delivery/receipt:** move goods into the destination stock and mark the order delivered exactly once.
7. **Settle:** at successful receipt, pay the seller for goods and charge the buyer for inbound freight; recognize seller revenue and seller COGS. Record inventory on the buyer side at landed acquisition cost.

Payments-on-delivery are a deliberately simplified first-release rule. Cash, order commitment, and availability MUST be validated so a buyer cannot promise the same funds to many suppliers. Stock commitments MUST also prevent double selling.

Orders that fail validation MUST produce an understandable, localized explanation and MUST leave the game state unchanged.

### 7.4 Delivery and lead time

Suggested deterministic formula:

`leadTimeDays = max(1, ceil(shortestRoadPathLength / logisticsTilesPerDay))`

`inboundFreightCents = baseFreightCents + pathLength * ratePerTileCents * quantity`

- Routing MUST respect valid road connections.
- If no transport path exists, the order MUST be unavailable or rejected before confirmation.
- Fees and speed MUST be scenario-configurable.
- Shipment status and estimated arrival MUST be visible in the Orders panel.
- First-release delivery is all-or-nothing; no partial shipments.

## 8. Demand, Markets, and Prices

### 8.1 Residential market

Residential districts generate the underlying household demand for consumable finished goods. Each residential cluster has population-weight, relevant product preferences, and a deterministic daily demand schedule.

A store’s reachable demand SHOULD depend on:

- Nearby household activity and accessibility.
- Retail price relative to competing offers.
- Product availability (no stock → no sale).
- Competing stores serving the same consumer market.

Demand MUST NOT create unlimited sales merely because a store has inventory. Total consumer purchases for a product in a cluster MUST NOT exceed that cluster’s daily demand quota.

A simple deterministic weighted-choice allocation across available stores is sufficient. Actual household characters and pathfinding are not required.

### 8.2 Price setting

- AI sellers publish unit-price offers for selected products.
- The player may compare supplier price, estimated freight, and delivery time before issuing an order.
- Store retail prices are editable subject to scenario-defined safe min/max ranges if needed.
- A lower retail price SHOULD increase a store’s relative demand capture, but MUST NOT guarantee profitability.
- Prices, quantities, and daily demand assumptions MUST be data-driven.

## 9. AI Competitors and Company Ecology

Release 1.0 uses **deterministic rule-based AI**, without paid generative models. The standard city contains 40 or more independently operating and trade-accessible AI companies, with diverse industries, product portfolios, prices, facility locations and purchasing preferences.

### 9.1 Operating behavior

AI firms MUST publish feasible offers and/or purchasing opportunities; respond to purchase orders; choose suppliers using cost, freight, reliability and lead time; replenish inventory; produce eligible recipes; compete for retail demand; and transact with player and AI customers. All decisions MUST issue the same typed, validated commands and obey the same economic constraints as the player. Strategies use configurable stock targets, price sensitivity, expansion thresholds and risk tolerance. Contention tie-breaks and AI evaluation order MUST be stable.

### 9.2 Founding, growth and closure

An AI firm's lifecycle MUST be explicit: emerging, active/stable, growing, struggling, insolvent, liquidating and closed (or equivalent documented states). Financial insolvency rules in §10 remain authoritative; the lifecycle is not a second competing accounting state.

- **Founding:** a deterministic market-opportunity process scores unmet demand, local competition, upstream inputs, prospective margin, site accessibility and initial capital. Entrants need a legal road-connected establishment and documented funded equity; never conjure stock or cash without ledger entries.
- **Growth:** companies may buy lots, open facilities, expand product ranges or integrate vertically through ordinary commands.
- **Retrenchment:** unprofitable firms may cut output, close facilities or liquidate assets; configurable consecutive-day thresholds prevent instant closures from one bad day.
- **Insolvency and liquidation:** companies with insufficient funding stop creating ineligible new commitments. Existing accepted orders, reserved cash, shipments, title and property remain traceable and are settled, cancelled or resolved via explicit deterministic liquidation rules. Closure never deletes history or duplicates stock/funds.
- **Market floor:** standard presets MUST support at least 40 trade-accessible AI firms after each completed simulation day, using a deterministically ordered, viable reserve entrant pipeline with scenario-documented initial equity. When an eligible replacement cannot be funded or located, validation MUST report an infeasible scenario rather than invent offers, cash or routes.
- **Persistence:** preserve founding/closure dates, company IDs, asset transfers, transactions, PRNG state and next entrants in the save/replay stream.

### 9.3 Bounded ecology evaluation

Evaluate market entry/exit using indexed sector/district demand, offers, accessible lots and event triggers on configurable cadences, not all tiles for every AI firm each day. Stagger expensive company plans while preserving seeded reproducibility. No AI uses future information or privileged economic shortcuts.

Market status changes are decided after financial posting; new entrants/facilities begin trading in the next day's order phase.

## 10. Finance and KPIs

### 10.1 Financial event rules

The game is an **educational simplified business ledger**, not a full ERP accounting package.

- Initial equity contribution increases cash and invested capital; it is not revenue.
- Purchasing land or buildings reduces cash and creates an asset; it is not immediately a P&L expense.
- Purchasing inventory reduces available cash at settlement but is not COGS until the inventory is externally sold.
- Manufacturing transfers input inventory cost and paid processing costs into output inventory cost.
- External deliveries and completed household sales recognize revenue and associated COGS.
- Recurring daily operating expenses reduce cash and period profit.
- Transport charges paid by the buyer are capitalized into acquired inventory landed cost for the core model.
- A company MUST NOT recognize revenue when it transfers stock between its own establishments.
- Initial scenarios MUST NOT produce negative cash; infeasible commitments must be prevented. Funds reserved for confirmed purchase orders are not spendable for construction, production, or discretionary purchasing.
- **Insolvency handling:** if a mandatory daily cash expense cannot be covered by uncommitted cash, the affected company enters an `insolvent` state, its unpaid expense is recorded as an obligation, and its operations are suspended; the player receives a clear end-of-run explanation. The engine MUST NOT silently reduce expenses or make cash negative.

### 10.2 Mandatory metrics

**Revenue (`faturamento`)**  
Sum of fulfilled external sales recognized within the reporting period. Unconfirmed orders and undelivered goods MUST NOT count.

**Net profit (`lucro líquido`)**  
`recognizedRevenue - COGS - recognizedOperatingExpenses` in the simplified core model. Since taxes, debt, and depreciation are initially out of scope, display a clear explanatory note that this is *simplified simulated net profit*.

**Return on investment (ROI)**  
`cumulativeSimplifiedNetProfit / cumulativeEquityContributed * 100`.

- ROI MUST show its numerator, denominator, period, and simplified definition when clicked or hovered.
- Cash balance MUST be distinct from net profit; asset purchases are a primary teaching case.
- Financial values MUST use integer BRL cents in the simulation, with deterministic rounding at monetary boundaries.
- Negative profit and negative ROI ARE valid results.

### 10.3 Operational metrics

- Orders placed, accepted, delivered, rejected, and cancelled.
- On-time delivery rate: delivered on/before promised day ÷ delivered orders, where the denominator is nonzero.
- Stockout days or unfulfilled household demand.
- Facility utilization based on configured maximum output.
- Inventory on hand, reserved inventory, and inventory valuation.
- Optional: inventory turnover and supplier lead-time averages when sufficient event history exists.

All metrics MUST be derived from the same source-of-truth state/events as the game simulation.

## 11. Simulation Time and Determinism

One button advances the simulation by one day. Optionally, the player can advance several days with a confirmation identifying outstanding orders. The player may pause indefinitely between days.

**Required daily processing order:**

1. Freeze commands selected for the next day and validate them.
2. AI companies create decisions in stable company-ID order.
3. Confirm/reject eligible orders and establish reservations.
4. Dispatch confirmed orders when ready.
5. Advance shipments and deliver those whose arrival date has been reached.
6. Process manufacturing/extraction using currently available inputs and capacity.
7. Allocate household demand and generate retail sales receipts.
8. Post daily operating expenses and business financial events.
9. Apply market ecology: review distress, entry, growth, retreat, liquidation and closure; validate the 40-counterparty minimum and schedule next-day activation.
10. Recalculate metrics and append a day summary and structured event log.
11. Advance the day counter.

A manufacturing output generated in step 6 MUST NOT be sold B2B in step 4 of that same day. For simplicity, orders submitted on day `d` can arrive no earlier than day `d+1`.

- The engine MUST advance time without React or DOM dependencies.
- The engine MUST NOT use `Math.random()` for outcome-critical decisions; use a seeded PRNG.
- Commands and events MUST have stable IDs and defined tie-break ordering.
- Replaying a scenario with the same seed/config/version/actions MUST produce identical order histories, inventory, and KPI results.

## 12. Main User Interface

### 12.1 Screens

**City / World**
- 320 × 240 tile viewport-rendered city map with visible district, lots, road hierarchy, establishment and ownership layers; selectable lot footprints and toggles for zoning, competition, access and land value.
- Property detail card showing zone, land value, activity index, building compatibility, and current owner.
- Context actions: purchase lot, construct eligible facility, inspect owner/facility.

**Company Dashboard**
- Revenue, simplified net profit, ROI, and cash.
- Daily trend chart, active establishments, and operating alerts.

**Orders**
- Tabs/views for `Purchases`, `Sales`, `Internal Transfers`, and `Shipments`.
- Order creation form, supplier comparison, order history, lifecycle/status, and issue explanations.
- Detail view MUST visually distinguish PO and SO roles without duplicate ledger entries.

**Facilities / Inventory**
- Facility capacities, on-hand and reserved stock, production recipe, sale prices, and product offers.
- Warnings for empty stock, overcommitted purchasing funds, or unavailable recipes.

**Market / Company Directory**
- Search and filter 40 or more trade-accessible companies by product category, supplier/customer role, district, published offers, requested goods, unit prices, freight and lead time.
- Select named AI suppliers or customer companies and inspect their establishments, availability, order history and company lifecycle.
- Expose newly founded and closed firms; historical firms cannot submit or accept fresh commercial orders.
- Supplier offers from AI and eventually the player.

**Learning / Missions**
- Current objective, short definitions, optional hints, and evidence of completed tasks.
- End-of-scenario report presenting both commercial results and operational behavior.

### 12.2 UI constraints

- Gameplay language MUST default to `pt-BR` (e.g., `Pedido de Compra`, `Pedido de Venda`, `Estoque`, `Faturamento`).
- Monetary formatting MUST use `R$` and Brazilian decimal/thousands conventions.
- Internal code and state MUST retain stable English field names.
- Zone meaning MUST use text/patterns/icons in addition to color.
- All essential actions MUST work with keyboard and pointer.
- Target desktop/laptop use in classroom; tablet usability SHOULD be maintained.
- The map MUST NOT be the only way to access an establishment; a facility list is required.
- Tooltips and glossary MUST explain terms such as PO, SO, lead time, landed cost, profit, and ROI.

## 13. Pedagogical Design and BNCC Mapping

### 13.1 Curriculum position

The BNCC governs essential learning in Brazilian Basic Education. This game is intended as a **pedagogical supplement** for a vocationally oriented course. Mapping an activity to a BNCC skill does not, by itself, certify a professional course or establish formal curricular compliance.

For the *Assistente Administrativo de Projetos* course, the teacher or institution SHOULD check the actual student age/education level, its course plan, and whether it is a formal professional and technological education offering. The **Resolução CNE/CP nº 1, de 5 de janeiro de 2021** provides an additional general reference for professional and technological education where applicable.

### 13.2 BNCC competencies and evidence

| Reference | Proposed learning application | Observable gameplay evidence |
|---|---|---|
| General Competence 2 — scientific, critical and creative thinking | Compare purchasing alternatives and diagnose operational outcomes | Supplier selection rationale and corrective decisions |
| General Competence 5 — digital culture | Use a digital business simulation and interpret its data | Orders, charts, exports, and dashboard interpretation |
| General Competence 6 — work and life project | Understand business functions and interdependencies | Planning of a small company and its growth |
| General Competence 7 — argumentation | Justify choices using financial/operational evidence | Short decision explanation in end-of-activity report |
| General Competence 10 — responsibility and citizenship | Evaluate resource use and responsible decisions | Avoiding waste and considering service commitments |
| EM13MAT101 | Analyze variation of economic quantities via charts and change rates | Compare revenue/cost trends and identify turning points |
| EM13MAT203 | Apply mathematical concepts to planning and decisions with digital tools | Use cost/price/budget information to choose suppliers and investments |
| EM13MAT303 (optional finance extension) | Work with percentages and interest-related calculations | Compare financing alternatives only if credit mechanics are added |

**Important:** These are designed instructional connections, not official lesson plans or a claim that every skill is completely assessed. EM13MAT303 SHOULD NOT be presented as fully achieved by the initial release without a dedicated financial mathematics activity.

### 13.3 Suggested classroom sequence

**Lesson 1 — Supply-chain discovery**  
Students identify product origin, supplier, manufacturer, warehouse, retailer, and end customer. Practical task: obtain the first product, create a PO, and explain each participant’s role.

**Lesson 2 — Purchasing and order tracking**  
Students compare unit price, freight, and delivery lead time. Practical task: place and track two orders, then calculate total landed cost and evaluate delivery reliability.

**Lesson 3 — Inventory and operating decisions**  
Students manage replenishment and a basic production recipe. Practical task: prevent a stockout while avoiding excessive inventory accumulation.

**Lesson 4 — Business performance and expansion**  
Students analyze revenue, simplified profit, ROI, cash, and investment. Practical task: present a concise evidence-based decision on whether to expand.

### 13.4 Teacher mode and evaluation

The initial teacher mode MUST allow selection of a preset containing:

- Seed and scenario configuration.
- Starting capital and facilities.
- Session duration (default: 30 game days).
- Accessible products and permitted actions.
- Learning objectives and assessment criteria.

A local end-of-session report MUST include:

- Scenario ID, seed, config version, and achieved day.
- Company metrics and operational KPIs.
- Orders issued/fulfilled and evidence of completing assigned objectives.
- Timestamped game events and a concise decision timeline.
- Optional student-entered reflection on supplier choice, stock, and investment.

**Suggested rubric:** order-process understanding (30%), supply/inventory decisions (25%), financial interpretation (25%), and evidence-based explanation (20%). This is a proposed starting rubric, not a BNCC-mandated weighting.

The teacher SHOULD be able to compare results under the same seed, while recognizing that the highest profit alone is not a sufficient assessment of learning.

## 14. Technical Architecture

### 14.1 Stack

- **Language:** TypeScript with strict type-checking.
- **UI:** React and Vite.
- **Rendering:** Canvas 2D with viewport-culling, chunk caches, a spatial hit-test index and local versioned sprite manifest; SVG is acceptable for small overlays, not 76,800 separate DOM tiles.
- **Styling:** responsive CSS; component library optional, not mandatory.
- **Simulation:** framework-independent TypeScript core library.
- **Testing:** Vitest for unit and simulation tests; Playwright is an optional end-to-end choice.
- **Storage:** browser IndexedDB for saves (localStorage acceptable for a very small proof of concept), plus JSON import/export.
- **Hosting:** static Vercel deployment with GitHub-based continuous deployment.
- **Backend:** none for the initial release.

Dependencies SHOULD be lightweight and justified. The simulation MUST NOT depend on proprietary cloud services or API usage charges.

### 14.2 Suggested repository layout

```text
/
├── SPEC.md
├── README.md
├── package.json
├── vite.config.ts
├── tsconfig.json
├── public/
├── src/
│   ├── app/                  # Browser bootstrap, providers, navigation
│   ├── ui/                   # Presentation components only
│   │   ├── city/
│   │   ├── dashboard/
│   │   ├── orders/
│   │   ├── facilities/
│   │   └── learning/
│   ├── simulation/
│   │   ├── engine/           # Scheduler, PRNG, command processor
│   │   ├── world/            # 320x240 city, road graph, districts, lots
│   │   ├── economy/          # Companies, offers, consumers, prices
│   │   ├── procurement/      # B2B orders, reservations, fulfillment
│   │   ├── inventory/        # Stocks, transfers, valuation
│   │   ├── production/       # Recipes and processing
│   │   ├── logistics/        # Distance, dispatch, transit, receipt
│   │   ├── finance/          # Ledger, accounting, KPIs
│   │   ├── ai/               # Firm strategies, market entry/exit
│   │   └── learning/         # Missions, objectives, results
│   ├── data/                 # Versioned multi-sector catalogs and scenarios
│   ├── assets/               # Sprite catalog and licensing manifest
│   ├── persistence/          # Save/load, migration, import/export
│   ├── localization/         # pt-BR strings and glossary
│   └── tests/
│       ├── unit/
│       ├── integration/
│       └── scenarios/
└── docs/                     # Optional implementation decisions
```

### 14.3 Architectural boundaries

- `simulation/*` MUST NOT import from `ui/*`, React, browser storage, or browser drawing APIs.
- Each domain module MUST own and validate its invariants.
- UI MUST issue typed commands; it MUST NOT mutate simulation state or financial balances directly.
- One authoritative state store MUST hold company, world, inventory, order, shipment, and financial data.
- Cross-module effects MUST occur via explicit engine orchestration and/or typed domain events.
- Scenario data MUST be versioned and validated at load time.
- Reusable operations (money arithmetic, IDs, PRNG, recipe math, reservations) SHOULD be implemented once.
- Avoid speculative generic abstraction frameworks; prioritize simple composable domain code.

### 14.4 Example commands and events

```ts
type GameCommand =
  | { type: 'SUBMIT_PURCHASE_ORDER'; buyerFacilityId: string; sellerFacilityId: string; productId: string; quantity: number }
  | { type: 'SET_RETAIL_PRICE'; facilityId: string; productId: string; unitPriceCents: number }
  | { type: 'BUY_LOT'; lotId: string }
  | { type: 'BUILD_FACILITY'; lotId: string; facilityType: 'store' | 'factory' | 'farm' | 'warehouse' }
  | { type: 'ADVANCE_DAY' };

type GameEvent =
  | { type: 'ORDER_CONFIRMED'; orderId: string; day: number }
  | { type: 'ORDER_REJECTED'; orderId: string; day: number; reason: string }
  | { type: 'GOODS_RECEIVED'; orderId: string; shipmentId: string; day: number }
  | { type: 'PRODUCTION_COMPLETED'; facilityId: string; recipeId: string; batches: number; day: number }
  | { type: 'RETAIL_SALE'; facilityId: string; productId: string; quantity: number; day: number }
  | { type: 'DAY_COMPLETED'; day: number };
```

These are illustrative interface boundaries. Implementation MUST also type internal-transfer, cash-commitment, cancellation, and financial-ledger events as required by actual workflows.

### 14.5 Performance and persistence

- The 320 × 240 city and 40+ counterparties MUST be tested on representative classroom hardware with recorded frame/time/memory budgets.
- Simulation updates MUST be batched; use cached road/district/sector indices, spatial culling and chunk redraws rather than rerendering the full map per event.
- If simulation time becomes significant, move engine advancement to a Web Worker without changing domain rules.
- A saved game MUST include generator/scenario/catalog versions, PRNG streams, current day, road/lot data, assets, active/historical companies, lifecycle and entrant state, pending orders and commitments.
- Save migrations MUST explicitly handle version changes or reject incompatible saves with a clear message.
- JSON imports MUST be schema-validated and treated as untrusted data.
- The initial game SHOULD work offline after its assets are loaded, but full PWA installation is not required.

## 15. Security, Privacy, and Accessibility

- The initial release MUST NOT require student email, name, login, or any personally identifying account data.
- A learner MAY export a local report and submit it to a teacher through the institution’s existing workflow.
- Exported files SHOULD use scenario IDs and optional voluntary aliases, not mandatory identity fields.
- Do not store confidential school records or secret API credentials in the client.
- External data import MUST enforce strict size and schema limits.
- UI MUST provide visible focus states, labels, readable contrast, and non-color status identification.
- Animations, if present, SHOULD respect reduced-motion preferences.
- The product MUST be playable without audio.

## 16. Verification and Acceptance Criteria

### 16.1 Core invariants (automated tests required)

- [ ] Same seed + scenario configuration + actions yields identical map, order history, and final KPIs.
- [ ] Every standard fixture generates 320 × 240 (76,800) tiles, all four zones, non-overlapping legal lots and a reachable road graph for all operating facilities.
- [ ] The player can discover and select at least 40 distinct, viable AI buyers and/or suppliers initially and at each completed day in the standard 30-day scenario.
- [ ] Entrants, firm growth/retrenchment, insolvency, liquidation and company closure produce stable historical IDs, correct financial/asset events and no disappearing pending orders.
- [ ] 320 × 240 map renders with viewport-culling, successful hit-tests and usable navigation/accessibility on representative school devices.
- [ ] Invalid building-zone combinations are rejected without state changes.
- [ ] Only an owning company can build on a lot.
- [ ] Two accepted orders cannot reserve the same stock unit twice.
- [ ] Two accepted orders cannot oversubscribe destination capacity or the buyer’s purchasing funds.
- [ ] Receiving the same shipment twice does not duplicate goods or payments.
- [ ] Rejected and cancelled pre-dispatch orders release reservations exactly once.
- [ ] Internal transfers preserve company-wide inventory value (apart from explicitly charged transfer cost) and do not generate revenue.
- [ ] Production consumes exact recipe quantities and does not create negative inventory.
- [ ] B2C sales do not exceed store availability or residential demand.
- [ ] A property purchase changes cash and assets but not immediate operating profit.
- [ ] A stock purchase changes cash and inventory but not immediate COGS.
- [ ] At delivery, a B2B sale records seller revenue and seller COGS once.
- [ ] Ledger-derived simplified net profit and ROI match the displayed dashboard.
- [ ] Player and AI commands follow the same business validity rules.
- [ ] Saving/loading preserves pending orders, shipments, commitments, and financial state.

### 16.2 End-to-end learning scenario

1. Create a game with a fixed classroom seed.
2. Choose cheese as the initial store product.
3. Inspect at least two supplier offers and their landed costs.
4. Issue a PO to an AI cheese factory and confirm the mirrored SO.
5. Advance time, follow shipment progress, and receive the goods.
6. Sell stock to residential consumers and verify the receipt/revenue ledger.
7. Inspect profit, cash, and ROI, explaining why they differ.
8. Purchase a valid lot, construct an eligible facility, and inspect the resulting change in cash/assets.
9. Receive or fulfill a B2B trade with another AI company.
10. Complete the 30-day activity and export a scenario report.

The release is accepted only if all ten steps work without manually editing game state.

## 17. Implementation Roadmap

**Release 1.0 is a single complete delivery, not a sequence of smaller public releases.** [ROADMAP.md](./ROADMAP.md) contains the implementation milestones M00–M09, and [TODO.md](./TODO.md) contains their issue-ready task IDs. Every system is designed from T0 for the full target.

- M00–M01: establish complete domain, schema, PRNG and event contracts for large-scale geography and evolving companies.
- M02: deliver seeded 320 × 240 terrain, roads, lots, districts, 40+ starting trade counterparties and geographic invariants.
- M03–M05: deliver stock, finance, procurement, logistics, production, demand and multi-category products.
- M06: deliver company AI plus entry, expansion, retrenchment, liquidation, closure and the minimum trading-market guarantee.
- M07–M08: deliver chunked map renderer, searchable company directory, accessible UI, local persistence and classroom assessment.
- M09: verify the full end-to-end economy, performance, deterministic replay, teacher paths and Vercel release.

## 18. Design Decisions and Open Questions

### Confirmed baseline decisions

- A single city and single player-run company per local game.
- At least 40 trade-accessible AI companies at each completed day in standard presets, with funded founding and explicit closure/lifecycle; count and strategy parameters remain configurable upward.
- All B2B trades require traceable orders; no instantaneous stock teleportation.
- Default city 320 × 240 tiles with roads, districts and variable-size lots; four zones and residential construction prohibited.
- Store/factory/farm/warehouse building types; companies may acquire, expand, retire and transfer facilities through recorded transactions.
- Revenue, simplified net profit, and ROI are headline metrics; cash is visible separately.
- Browser-only TypeScript implementation on GitHub/Vercel; Canvas 2D chunk rendering and verified local assets; no paid AI or cloud dependency.
- Seeded deterministic simulation and replayability.
- Brazilian Portuguese gameplay and English technical artifacts.

### Open questions to validate with the teacher

1. **Learner profile:** age group, prior mathematical knowledge, and official course modality.
2. **Lesson duration:** total classroom hours and whether each game day corresponds to an exercise segment.
3. **Production realism:** whether land/resource productivity should vary by location or remain recipe-only initially.
4. **Scenario balance:** starter capital, retail gross margins, property prices, daily demand, and difficulty presets.
5. **Contracts:** whether recurring supply contracts should be a second teaching module after basic PO/SO mastery.
6. **Assessment:** whether teacher reports need a printable format in addition to JSON/CSV.

None of these questions blocks the initial technical milestone; use configurable defaults until validated.

## 19. Reference Documents

- **BNCC — Ministério da Educação (official):** https://basenacionalcomum.mec.gov.br/ and https://www.gov.br/mec/pt-br/cne/bncc_ensino_medio.pdf
- **BNCC open/downloadable datasets (MEC):** https://downloadbncc.mec.gov.br/
- **Resolução CNE/CP nº 1, de 5 de janeiro de 2021 — General National Guidelines for Professional and Technological Education:** https://portal.mec.gov.br/dmdocuments/rcp001_21.pdf

**Provenance note:** Educational mappings in §13 were derived from the official BNCC material, including high-school mathematics codes EM13MAT101, EM13MAT203, and EM13MAT303, and from the identified professional education guidelines. Formal adaptation requires verification against the school’s curriculum and up-to-date applicable requirements.

---

**Document ownership:** Project team / educator.  
**Next artifact:** a small set of versioned scenario/product JSON fixtures and a focused GitHub issue plan derived from Milestone A, not a parallel specification system.