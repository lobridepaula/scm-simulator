# M00-S01 — STORY_001: Release 1.0 Requirements Traceability

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
- **Existing tracking Issue:** [#1 — M00-01 Requirements Traceability](https://github.com/lobridepaula/scm-simulator/issues/1)
- **Stable work IDs:** M00-T01, M00-T02; cross-release F01–F16, J01–J05.
- **Priority / dependency:** first foundational Story; no predecessor Issue. Gate G1 precedes decision finalization and implementation acceptance for #2 and #3.
- **Value objective:** make every committed Release 1.0 requirement accountable to a domain owner, milestone, existing task/Issue and independently inspectable verification oracle without altering the approved product contract.
- **Proposed executor:** Project Manager / requirements-documentation specialist; **independent reviewer:** Quality Auditor. Roles are proposed only; no plugin or developer was invoked or assigned in GitHub.

### In scope

1. Enumerate atomic requirements from SPEC §§3–16, classifying MUST, SHOULD, optional and explicitly excluded requirements; keep source anchors stable and record conflicts/ambiguities.
2. Create **one non-authoritative traceability index** (candidate: `docs/quality/REQUIREMENTS_TRACEABILITY.md`) whose rows link SPEC clause/anchor, FEATURES Fxx/subfeature, TODO Mxx-Tyy, existing Issue, owning domain from ARCHITECTURE §6, M00–M09 milestone, verification oracle/test evidence and relevant learning result; use `not applicable` with justification where appropriate.
3. Cover F01–F16, J01–J05, SPEC §16 invariants and classroom journey, preserving the full 160×120 world, 60+ SKUs / 10+ sectors, 50 initial AI firms, 40+ feasible trading counterparties and deterministic finance/engine constraints.
4. Clearly distinguish **planned**, **implemented**, **independently verified** and **released**; unresolved links and uncovered requirements are explicit rather than silently treated as complete.
5. Check README/SPEC §17/ROADMAP/TODO consistent with one complete Release 1.0; propose any corrections for separate approval, avoiding duplicate source-of-truth rules.

### Out of scope

Changing business rules, accepting ADRs, moving feature scope to later releases, inventing test IDs/passes, implementing the runtime or creating duplicate GitHub Issues.

### Acceptance and independent verification gates

- **A1:** reviewable matrix includes every relevant atomic clause of SPEC §§3–16, with requirement class and precise anchor; independent reviewer samples all sections and verifies no mandatory omissions.
- **A2:** every mandatory row resolves to an existing feature, stable TODO task, module owner, milestone and verification method, or carries an explicit gap requiring follow-up; check bidirectionally F01–F16 and SPEC §16.
- **A3:** no implemented, verified or released claim exists without a matching source SHA, actual test execution and result; planning-only entries remain marked planned.
- **A4:** repository-backed link integrity / duplicate row key / orphan-ID checks and negative case fixtures are specified; automated validation runs only when tooling exists and actual results are attached.
- **A5:** code-reviewed commit/PR plus independent QA coverage verdict; no issue close until gates and relevant M00 verification are evidenced.
- **G1 join criterion:** the reviewed and source-linked traceability baseline exists, mandatory gaps are reported and enough shared interfaces are mapped to allow #2/#3 decision work. G1 is **not yet met**.

### Handoffs, resource locks and open decisions

- Pass this **same file** to Task Coordinator for filling `## Task List` (no separate TASK_LIST.md). Task Coordinator must preserve this Story Context.
- #2 and #3 may perform read-only option research in parallel; they cannot claim the G1 gate before review.
- Candidate matrix is an index, not a second normative SPEC. Source contradictions go to the Product Owner for arbitration; none are pre-approved.
- Avoid concurrent edits to SPEC/FEATURES/ARCHITECTURE with #2/#3. Register the proposed matrix path after confirming no existing equivalent.

## Task List

_Pending Task Coordinator decomposition. No task dispatched, accepted or verified._
