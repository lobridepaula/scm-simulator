# Project Risk Register — Supply Chain Simulator

**Status:** Initial identified exposures for assessment; not quantified or accepted by the Product Owner.

## Assessment policy

Risk entries describe plausible failure modes grounded in [SPEC.md](../../SPEC.md), [ARCHITECTURE.md](../../ARCHITECTURE.md) and [ROADMAP.md](../../ROADMAP.md). Probabilities, impacts, risk owners and residual exposure require explicit assessment; no fabricated numeric scores are provided. Reassess before the affected milestone and after a material architectural or gameplay change.

## Identified risks

### RISK-001 — City feasibility and market density
- **Event:** The 160 × 120 map cannot legally fit 50 starting AI firms, road-connected lots, residential demand, four zones and expansion sites while maintaining 40+ feasible counterparts.
- **Consequence:** Invalid world seeds, unreachable partners or reduced trade choices.
- **Mitigation:** Validate generator capacity and market participation across multiple seeds; fail infeasible configurations explicitly.
- **Milestones / task references:** M00–M02; TODO M02-T01/T07/T09/T11.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

### RISK-002 — Economic conservation or double settlement
- **Event:** PO/SO mirroring, reservations, delivery or liquidation duplicates or destroys cash, stock or obligations.
- **Consequence:** Incorrect teaching outcomes, impossible balances and broken ROI/profit.
- **Mitigation:** One canonical transaction authority, event idempotency, money-in-cents, ledger reconciliation and contention tests.
- **Milestones:** M00, M03–M06; see SPEC §§7, 10 and 16.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

### RISK-003 — Nondeterministic AI and replay drift
- **Event:** Randomness, unstable tie-breaking, incomplete save state or changing catalog versions yield divergent classroom results.
- **Consequence:** Unreproducible comparisons, test flakiness and incompatible saves.
- **Mitigation:** Partitioned seeded PRNG streams, stable ordering, versioned fixtures and save/load replay regressions.
- **Milestones:** M00–M01, M06, M08–M09.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

### RISK-004 — Classroom-device performance
- **Event:** Canvas redraws, road pathfinding or 50-company daily AI execution stall the UI.
- **Consequence:** Unusable experience on school hardware.
- **Mitigation:** Chunk/viewport culling, cached indices, profile representative devices and introduce a Worker only if evidenced.
- **Milestones:** M02, M06–M07, M09.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

### RISK-005 — Scope, educational fidelity and accessibility
- **Event:** Incremental implementation is mistaken for a reduced MVP, or screens and metrics obscure decision consequences, pt-BR wording or keyboard workflows.
- **Consequence:** Incomplete Release 1.0 or misleading learning assessment.
- **Mitigation:** Feature-to-evidence traceability, teacher-seed acceptance, end-to-end exercises and non-color keyboard-accessible interactions.
- **Milestones:** M00, M07–M09.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

### RISK-006 — Local data and imported content
- **Event:** Unsupported/corrupt saves or hostile imports compromise progress, integrity or browser rendering.
- **Consequence:** Lost classroom work, invalid outcomes or client-side vulnerabilities.
- **Mitigation:** Versioned save envelopes, schema/size checks, atomic restore, safe string rendering, portable exports and invalid-input tests.
- **Milestones:** M01, M08–M09.
- **Probability / impact / owner:** To assess / to assess / {{RISK_OWNER}}.

## Review and escalation

- **Risk governance owner / cadence:** {{RISK_GOVERNANCE}}.
- Report changed exposure at affected milestone reviews; convert mitigations to traceable [TODO.md](../../TODO.md) tasks or GitHub Issues, without duplicating the execution backlog.
- Risks that would violate mandatory SPEC invariants require a design or scope decision by the Product Owner; they are not silently accepted through a workaround.
