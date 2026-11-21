# M00-S01 — STORY_003: Core Architecture Decision Records

Stage: **story-published**  
Document revision: **1**  
Last editor: `scrum-master`  
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
- **Existing tracking Issue:** [#3 — M00-03 Core Architecture and Runtime ADRs](https://github.com/lobridepaula/scm-simulator/issues/3)
- **Stable work IDs:** M00-T03/T04; FEATURES F01, F11, F12; ARCHITECTURE §§4–8, 11, 19, SPEC §§7, 9–11, 16.
- **Priority / dependency:** predecessor #1 G1 for decision finalization; shared financial interfaces require #2 G2-fin reconciliation. Read-only architecture inventory/options may proceed in parallel with #1/#2.
- **Value objective:** establish a precise full-system, browser-independent, deterministic architecture contract that enables later modules without competing state writers or premature engine shortcuts.
- **Proposed executor:** Software Architect specialist; **independent reviewer:** Quality Auditor; **cross-review:** finance/domain specialist for settlement and liquidation. Not actually dispatched.

### Already authoritative — protect these invariants

ARCHITECTURE §§4–8 defines the domain module dependency direction and canonical field ownership, command/event engine atomicity, typed UI-to-engine commands, read-only projections, no simulation import of React/DOM/Canvas/persistence, stable seeded PRNG streams, deterministic IDs and serialization intent. SPEC §11 fixes exactly **11 ordered daily phases** and player/AI command parity. Do not present these as new discretionary choices.

### In scope

1. Produce concise ADRs for unresolved envelope/ID namespaces, deterministic command/event tie-breakers, canonical serializer and hash, schema/save migration, and UI pre-day queue edits/validation.
2. Specify explicit module interfaces/owners and a cycle-free dependency graph covering engine, world, economy, procurement, inventory, production, logistics, finance, AI and learning; propose verifiable import/ownership fitness tests.
3. Document cross-module atomic transactions, ordered event publishing and replay validation; preserve the eleven phases and the d→d+1 delivery floor.
4. Co-design liquidation, in-transit custody and pending-order interfaces with #2; define world road graph indexing, renderer chunks and routing cache alternatives without inventing benchmark outcomes.
5. Record options, consequences and rejected alternatives with **Proposed** versus PO-approved **Accepted** states using existing `docs/architecture/DECISIONS.md` index and `docs/adr/` records; reconcile contracts only after approval.

### Out of scope

A second source of architectural authority, replacing accepted structure, introducing services/backends or paid AI, writing unverified runtime code, running speculative benchmarks, or inventing numeric performance thresholds.

### Acceptance and independent verification gates

- **A1:** architecture ADRs have source anchors, alternatives, consequence/risk analysis, an unambiguous approval record, and crosslinks to normative owner/interface docs; open choices remain Proposed.
- **A2:** document or testable-contract assertions prove a single writer for every canonical concept, no cycles, and browser-independent simulation; no direct UI-owned economy state.
- **A3:** deterministic order, ID, serialization/hash and player/AI validation test oracles include negative cases, replay consistency and save/load with outstanding orders.
- **A4:** shared custody, liquidation and settlement boundaries are consistent with #2 accepted decisions and SPEC; recorded trade-offs approved by the Product Owner.
- **A5:** road/chunk design selection cites actual measurements when measurement-dependent, otherwise remains explicitly pending; reviewed PR and independently verified checks are linked before closure.

### Dependencies, concurrency and decision gates

- **G0:** read authoritative baseline (satisfied for research).
- **G1:** reviewed #1 traceability (pending for finalization).
- **G2-arch:** explicit Product Owner approval for material open structure/serialization/queue/trade-offs (pending).
- **G3-shared:** reconciliation with #2 financial custody/liquidation interface decisions and ordered single-writer edits to `ARCHITECTURE.md`, `INTERFACES.md`, `DECISIONS.md` (pending).
- Separate option research may run in parallel with #2. Do not claim parallel source writes safe without branch/write-set review.
- Send this same Story to Task Coordinator for task list entry after registration; no competing task tracker.

## Task List

_Pending Task Coordinator decomposition. No task dispatched, accepted or verified._
