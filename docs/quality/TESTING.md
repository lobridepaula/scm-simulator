# Testing and Verification — Supply Chain Simulator

**Status:** Planned quality strategy. No test suite, CI pipeline or passing test result is currently evidenced.

## Quality objective

Prove the **complete** Release 1.0 as a functioning educational economy, not a collection of separately plausible screens. The authoritative invariant checklist is [SPEC §16](../../SPEC.md#16-verification-and-acceptance-criteria); observable capability checks are in [FEATURES.md](../../FEATURES.md). Milestone exit gates belong to [ROADMAP.md](../../ROADMAP.md).

## Test layers

| Layer | Planned verification |
| --- | --- |
| Static architecture | Strict TypeScript, lint, forbidden browser/UI imports into simulation, dependency cycles, schema compatibility |
| Domain unit | Money-in-cents/rounding, lot zoning, recipe arithmetic, IDs, deterministic PRNG, capacity and inventory arithmetic |
| Commands | Authorization, atomic rejection, commitment reservations, shared validation for player and AI, idempotency |
| Domain integration | One PO/SO order, buyer/seller quotation, shipment custody, dispatch, receipt, one settlement, internal transfers |
| Economy scenario | 60+ connected SKUs/10+ sectors, 50 initial AI firms, 40+ viable options (15 suppliers / 15 B2B buyers), seeded 30-day market entry/exit |
| Save and import | Reproduce a suspended in-transit game; migrations/rejections; hostile or malformed JSON size/schema bounds |
| Browser UX | End-to-end cheese lesson, supplier/customer selection, keyboard focus, pt-BR, screen-reader/non-color feedback and exports |
| Performance and publication | Representative classroom hardware measurements for 160 × 120 rendering and daily simulation; asset licenses, production build, deployment smoke test |

## Evidence and traceability

Each accepted test should have a stable ID assigned once implemented; link it to the existing FEATURES Fxx behavior, SPEC section, TODO Mxx-Tyy item, test path, committed revision, CI run and observed result. **No test IDs or passes are claimed yet.**

Required reference scenarios:

- Same seed + scenario/catalog/generator versions + commands → identical final snapshots, event history and KPI hashes.
- Multiple city seeds → connected routes, no overlapping lots, all zones, enough legal space and feasible trading firms.
- Competing orders → no double reservation of stock, available funds or destination capacity.
- Accepted AI purchase request → a single canonical B2B order and one settlement; rejected request → no state mutation.
- Property purchase → cash decreases and asset increases without immediate operating-profit loss.
- Saved mid-shipment session → same future behavior as uninterrupted simulation.
- Full 30-day classroom activity → student explains cash, profit, ROI and supplier choices using exportable evidence.

## Gates and tools

- At M00, establish scripts, fixtures and CI. Planned **Vitest** unit/integration/scenario coverage; **Playwright** is the preferred browser E2E adapter, subject to confirmed setup.
- A review may pass only with relevant evidence for changed behavior and dependencies; include architecture fitness checks.
- M09 must verify all mandatory SPEC §16 items and the end-to-end learning scenario. A green build alone is insufficient.
- Record benchmark device, browser, dataset, seed, cold/warm frame timing, step duration and memory; derive thresholds from actual representative devices, not invented budgets.
- Test imported JSON as untrusted input; validate invalid types, oversized inputs, hostile text, incompatible versions and interrupted writes.

**Verified runnable commands:** none (documentation-only repository). After scaffold, confirm package scripts instead of assuming they exist.

**Open decisions:** test fixture/version policy, classroom benchmark hardware and CI service configuration. Track implementation under [TODO.md](../../TODO.md).
