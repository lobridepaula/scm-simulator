# ADR-M00-002 — Pre-Day Command Queue and Atomic Transactions

**Status:** Proposed — not accepted. **Owner:** Software Architect. **Approver:** Product Owner pending. **Issue/Story:** #3 / TL-003. **Baseline:** main `158d2773bc3ae40997185571a0c3319c619ea297`.

## Invariants already fixed
[SPEC §11](../../SPEC.md) and [ARCHITECTURE §§7–8,11](../../ARCHITECTURE.md): the player may pause between days, day advancement executes exactly eleven specified phases, freeze/validation precedes AI decisions, both player/AI use the same command processor, a rejected command changes no canonical domain state, cross-domain effects and events commit atomically, order earliest delivery is d+1, and phase-6 outputs cannot be dispatched in phase 4 of that day. UI is a read-only projection with typed commands. Nothing here changes these rules.

## Open interaction options
1. **Editable pending queue:** A — allow update/remove of a not-yet-frozen queued intent by original command ID with deterministic revision counters; B — immutable queued submissions plus explicit cancel/replace intent. Either must specify when and how duplicate IDs, stale revision, day mismatch and conflicts reject. No option selected.
2. **Validation before day advance:** A — display optimistic *advisory* validation on enqueue and mandatory authoritative recheck at phase 1; B — enqueue syntactic validation only and evaluate all economic feasibility at phase 1. Either uses canonical state at the commit boundary; advisory state is never a stock/cash reservation.
3. **Concurrent UI/AI intentions:** A — assign engine queue sequence at submission then deterministic phase-1 stable sort; B — freeze an ordered immutable batch and assign canonical sequence at freeze. AI commands are generated in company ID order in phase 2, not inserted as user queue events.

## Transaction boundary (architectural requirement)
`validate -> stage all domain changes -> check invariants -> commit new state+event batch+deduplication keys OR reject everything`. The engine coordinates procurement order, inventory outbound/inbound commitments, finance funds, and logistics shipment; no listener can initiate independent settlement. A failed receipt must leave inventory, finance and order lifecycle unchanged together.

## Transition oracles (not executed)
- Queue edit before freeze changes only pending intent presentation; same old ID/revision cannot be applied twice.
- Queue edit after freeze, duplicate ID, invalid issuer, stale day, rejected quote, insufficient funds/capacity/stock => no partial domain state/event updates.
- Two competing POs committing identical stock or spendable funds in one phase must have stable winner/loser ordering, never overcommit.
- Same seed and command batch with different React render cycles => same receipt/ledger hashes.
- Phase 4 before phase 6 prevents same-day dispatch of manufactured stock; receipt floor d+1.
- Advance multiple days invokes the same single-day transition repeatedly, not a second rule path.

## Trade-offs and gate
Editable queues have higher UI flexibility and more identity/versioning complexity; immutable revisions are simpler to replay but require clearer UX. Optimistic previews improve feedback but can mislead unless marked non-authoritative. PO must approve actual editing and validation semantics; Software Architect can implement neither option as settled policy until approved. Dependent typed contracts may be sketched in documentation, not declared final.
