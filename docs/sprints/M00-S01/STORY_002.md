# M00-S01 — STORY_002: Accounting and Financial Decisions

Stage: **tasks-populated**  
Document revision: **2**  
Last editor: `task-coordinator`  
Registration: **pending** (review and Task Coordinator handoff)

## Story Context

- **Repository:** [lobridepaula/scm-simulator](https://github.com/lobridepaula/scm-simulator)
- **Sprint:** M00-S01 — Product Contract and T0 Foundations (first development Sprint)
- **Target integration branch / inspected baseline:** `main` / `0b0046d45b577ba4db3de35c2d220440444db634`
- **Product Owner:** repository owner (business and substantive architecture decisions require explicit approval)
- **Release constraint:** Full Release 1.0 from T0, M00–M09 gates, no MVP or temporary engine.
- **Primary authority:** [SPEC](../../../SPEC.md) for behavior, [FEATURES](../../../FEATURES.md) for observable behavior, [ARCHITECTURE](../../../ARCHITECTURE.md) for structure, [ROADMAP](../../../ROADMAP.md) for milestones, [TODO](../../../TODO.md) for stable tasks, [AGENTS](../../../AGENTS.md) for change control.
- **Evidence classification:** inspected planning baseline only. There is no runnable scaffold, verified CI, implementation PR, or independently validated acceptance evidence at kickoff.
- **Execution authority:** creation of reviewable Sprint planning artifacts only. No agent dispatch, domain implementation, accepted policy, merge, release or Issue closure is implied.
- **Existing tracking Issue:** [#2 — M00-02 Accounting and Delivery Decisions](https://github.com/lobridepaula/scm-simulator/issues/2)
- **Stable work IDs:** M00-T03/T04; FEATURES F05, F08, F12; SPEC §§6–7, 10, 16; ARCHITECTURE §§6, 11, 19.
- **Priority / dependency:** hard predecessor #1 / reviewed G1 traceability baseline for decision finalization and dependent implementation. Read-only alternatives analysis may start without G1.
- **Value objective:** enable one auditable inventory/cash/ledger/receiving policy, with no duplicate settlement, cash overcommitment or silent valuation differences.
- **Proposed executor:** finance/domain-analysis specialist with Software Architect for cross-domain seams; **independent reviewer:** Quality Auditor. This is a role recommendation, not an actual dispatch.

### Already authoritative — do not silently reopen

SPEC §§7, 10 require payment and seller revenue/COGS on successful receipt; buyer pays and capitalizes inbound freight into landed inventory cost; property purchase capitalizes assets; initial equity is not revenue; confirmed POs reserve total funds; cash cannot become negative; insolvent firms carry unpaid mandatory obligations; internal transfers are not sales; B2B PO/SO share one canonical order; settlements and receipts are exactly once. Any contrary alternative requires explicit SPEC change authorization.

### In scope

1. Assess alternative inventory cost-flow methods (e.g., weighted average/FIFO), integer-cent rounding/allocation on purchase, production, shipment and consumption, and transfer valuation with conservation examples.
2. Separate legal title, physical custody, revenue recognition and receivable/payment points during dispatch, transit, receipt, cancellation and liquidation. Preserve already-specified settlement-at-receipt.
3. Define funds-commitment release/reconciliation, receipting idempotency keys, freight charge postings, insolvency obligations and deterministic resolution of pending orders, in-transit stock and assets.
4. Draft decision records as **Proposed** in the existing `docs/adr/` convention, with alternatives, rejected choices, consequences, test oracles, affected SPEC/ARCHITECTURE/INTERFACES and named PO approval status.
5. After explicit PO approval, reconcile the normative documents and test contracts through a reviewed change; do not accept ADRs by drafting them.

### Out of scope

Deploying financial behavior before policy approval, a second settlement engine, UI-derived ledger values, unapproved changes to payment-on-receipt, full ERP/tax/debt additions, or closing the existing Issue without verified evidence.

### Acceptance and independent verification gates

- **A1:** each open financial trade-off has a comparison of alternatives, deterministic examples, invariant and cost/implementation consequences, and proposed/rejected status clearly identified.
- **A2:** explicit recorded Product Owner decisions approve the selected material financial policies before ADR acceptance or dependent domain implementation.
- **A3:** accepted decisions match SPEC, ARCHITECTURE, INTERFACES, single-order ownership and cash/inventory/ledger authority; shared liquidation/custody interfaces agreed with #3.
- **A4:** test oracles cover exact-once receipt and posting, competing cash/stock/capacity commitments, cancellation/rejection release, landed cost, COGS, negative-profit-valid cases, insolvency and save/replay. Include invalid/boundary examples.
- **A5:** reviewed branch/PR and actual automated tests/CI when available; distinguish blocked-by-scaffold from passing. Independent QA required before Issue closure.

### Dependencies, write locks, decisions

- **G0:** authoritative requirements and baseline available (satisfied for read-only research).
- **G1:** #1 traceability independently reviewed (pending); required for finalized decisions and integration.
- **G2-fin:** Product Owner signs off inventory costing, rounding, custody/title, commitment and liquidation trade-offs (pending).
- Shared files `ARCHITECTURE.md`, `docs/architecture/DECISIONS.md`, and interface descriptions are a **single-writer integration lock** with #3. Separate read-only analysis and isolated proposed ADR branches are compatible; concurrent edits to shared files are not.
- Ask Product Owner only about genuinely open choices; buyer freight responsibility and receipt recognition are already contracted.

## Task List

| TL-ID | Responsible plugin | Concrete action | Expected deliverable | Independent reviewer | Status | Evidence / blocker |
|---|---|---|---|---|---|---|
| TL-001 | Project Manager | Extract fixed versus unresolved rules from SPEC §§6–7, 10, 16 and ARCHITECTURE §§6, 11, 19; distinguish procurement/finance/inventory/logistics authority; do not reopen accepted receipt terms. | Source-linked accounting decision inventory and conflict list for #2, with fixed constraints marked non-negotiable. | Quality Auditor | To do | [#2](https://github.com/lobridepaula/scm-simulator/issues/2); read-only G0 permitted while #1 runs. |
| TL-002 | Software Architect | Compare FIFO and weighted-average inventory costing, integer-cent rounding, input-to-output production costs, transfers, landed freight and COGS using conservation examples. | Alternatives table with deterministic numerical examples, invariants, rejected-path consequences and test oracles. | Quality Auditor | To do | Depends on TL-001; research only; policy not selected. |
| TL-003 | Software Architect | Analyze title versus physical custody at dispatch/transit/receipt, cash commitment/release, idempotent settlement, insolvency, pending obligations and liquidation handoffs with #3. | Source-linked state-transition and ownership alternatives with duplicate-receipt, cancellation and insolvency negative cases. | Quality Auditor | To do | Depends on TL-001; coordinate read-only interfaces with STORY_003; no shared-file writes. |
| TL-004 | Project Manager | Consolidate TL-002/TL-003 options into explicit decision requests and submit policy trade-offs for Product Owner approval only after reviewed #1 G1. | PO decision packet listing each choice, recommendation rationale, invariant impact and explicit approval/unknown status. | Quality Auditor | Blocked | G1 #1 reviewed traceability pending; TL-002 and TL-003 outputs pending; no PO approval. |
| TL-005 | Software Architect | Prepare source-linked ADR proposals for unresolved accounting and delivery policies in the existing docs/adr convention, marking all unapproved options Proposed. | Reviewable proposed finance ADRs with considered options, consequences, source links, test oracles and pending approver. | Quality Auditor | Blocked | Depends on TL-002–TL-004; ADR file writes require separate approved specialist execution; no accepted policy. |
| TL-006 | Project Manager | After explicit PO approval, reconcile only affected SPEC, ARCHITECTURE, INTERFACES and DECISIONS references; coordinate a single-writer integration with #3. | Authorized change set or PR with approved decision links and no duplicated financial authority. | Quality Auditor | Blocked | G1 + PO approval + TL-005 + #3 shared-interface agreement and serialized write lock required. |
| TL-007 | Quality Auditor | Independently check approved accounting decisions, order/ledger/custody invariants, documented test oracles and actual executable results at source SHA; report unavailable CI separately. | Independent accounting verification report with criterion-level evidence, failures and closure recommendation. | Scrum Master | Blocked | Requires TL-006 reviewed changes, available test environment/CI where applicable; no test results yet. |

**Handoff:** Tasks are allocated to verified plugin roles for proposed execution only. Scrum Master owns dispatch and Story acceptance; PO approval and branch-write authorization remain separate. Task Coordinator has not invoked any specialist.
