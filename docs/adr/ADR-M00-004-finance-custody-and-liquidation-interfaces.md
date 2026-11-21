# ADR-M00-004 — Shared Finance, Custody and Liquidation Interfaces

**Status:** Proposed cross-domain contract / not accepted. **Owners:** Software Architect with #2 financial specialist. **Decision approver:** Product Owner (pending). **Independent verifier:** Quality Auditor (pending). **Finance cross-review:** STORY_002 owner (pending). **Issue/Story:** #3 TL-005/TL-006; predecessor #2. This document does not settle accounting policy.

## Fixed requirements
[SPEC §§7,10](../../SPEC.md), [ARCHITECTURE §§6,11,19](../../ARCHITECTURE.md), [STORY_002](../sprints/M00-S01/STORY_002.md): one procurement-owned `CommercialOrder` with PO/SO projections; finance owns funds, committed cash, assets and ledger; inventory owns on-hand, outbound reservations, inbound capacity and cost basis; logistics owns shipment/custody/arrival eligibility; engine is sole cross-domain committer. Payment, seller revenue and COGS at successful receipt; inbound freight buyer-paid and capitalized; no negative/overcommitted spendable cash; exactly-once receipt/settlement; insolvency retains mandatory unpaid obligations; internal transfers are not sales.

## Open policy questions owned by #2 / PO
1. Inventory cost flow (weighted average or FIFO or another explicit deterministic policy) and BRL-cent allocation/rounding across split lots, production and transfers.
2. Ownership/title versus physical custody from dispatch to successful receipt; risk allocation during transit and liquidation.
3. Failed/cancelled/pre-dispatch order commitment release, in-transit pending obligations, property and outstanding liability treatment at liquidation.
4. Precise settlement/receipt idempotency key namespace, while retaining successful-receipt posting timing.

## Boundary options (not a choice)
- **Reservation API:** A — grouped `prepare/commit` value transitions for procurement, finance and inventory coordinated by engine; B — pure functions returning candidate updates/events which engine validates and applies in one immutable commit. Both require no partial mutation, stable order-key correlation and no second settlement.
- **Custody reference:** A — shipment record references order+inventory transit batch with explicit physical custodian and separate title holder; B — a distinct engine-owned transit register with logistics-owned shipment state. Must avoid two writers for physical stock and in-transit value; any cross-domain ownership rule must be explicitly specified.
- **Liquidation coordinator:** A — engine runs one ordered liquidation transaction applying explicit per-domain transition proposals; B — deterministic multi-step liquidation state machine with saved checkpoints and no cross-domain partial commits. No assumed destruction of goods, forgiveness of debt or ownership transfer.

## Required coordinated event/test oracles (not executed)
`ORDER_CONFIRMED`: procurement order and matching inventory/funds/capacity commitments atomically or none.
`GOODS_DISPATCHED`: outbound commitment and physical stock-to-transit custody remain conserved; finance recognition does not occur prematurely.
`GOODS_RECEIVED`: once-only destination physical stock/cost basis, matching buyer cash and seller revenue/COGS, inbound freight capitalization and posting IDs; replay/duplicate transport events cannot repeat effects.
`ORDER_CANCELLED` before dispatch: deterministic release of exactly the associated reserved stock, inbound capacity and funds; no artificial revenue.
`COMPANY_LIQUIDATION`: pending order/transit/assets/obligations are handled under PO-approved policy and remain audit-visible, including across save/load.
Negative tests: duplicate receipt; lost shipment route; capacity shortfall; competing orders; default during transit; invalid inventory cost layer; insufficient funds; mid-liquidation save/load; negative profit (allowed) versus negative spendable cash (not allowed).

## Blocker and next required receiver
This is a *coordination interface proposal*, not an accepted ADR. #2 must supply PO-approved valuation/title/liquidation choices before `ARCHITECTURE.md`, `INTERFACES.md` or settled simulation types are changed. Shared-file edits are serialized. Report unresolved choices, do not silently use implementation defaults.

## Decision record and authority

- **Decision date:** Pending; this proposal has no approval date.
- **Selected alternative:** None — Product Owner decision pending.
- **Rejected alternatives:** None of the viable A/B/C alternatives rejected; only constraints explicitly prohibited by SPEC/ARCHITECTURE are invalid. Rejection rationale is pending the decision.
- **Supersedes:** None.
- **Decision log:** Proposed for M00-S01 STORY_003 / Issue #3; no accepted or superseded revision. Future approval must name the chosen option, approver, date, rationale, rejected alternatives and impacts.
- **Affected authorities:** `SPEC.md` §§7,10,16; `ARCHITECTURE.md` §§6,11,19; `docs/architecture/INTERFACES.md`; `docs/sprints/M00-S01/STORY_002.md`; `docs/architecture/DECISIONS.md`. G2-fin and coordinated G2-arch approval are pending. The existing normative documents remain unchanged pending approved reconciliation.
- **Verification status:** **NOT RUN** — proposed oracles are specifications, not test definitions executed in CI; no application scaffold or runnable architecture check is present on the inspected baseline. Regression evidence, integration SHA and any later performance measurements must be recorded separately.
