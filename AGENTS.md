# Agent Working Agreement — Supply Chain Simulator

> Repository-specific instructions for automated development agents. Higher-priority host instructions, security policies, and explicit Product Owner decisions still apply. This file does not grant repository access.

## Before any change

1. Read [README.md](README.md) to understand the project and its pre-implementation status.
2. Read [SPEC.md](SPEC.md) for approved gameplay, education, scope, numeric constraints and economic semantics.
3. Read [FEATURES.md](FEATURES.md) for the complete observable Release 1.0 behavior and acceptance criteria.
4. Read [ARCHITECTURE.md](ARCHITECTURE.md) for normative module boundaries, state ownership, scheduler and integration design.
5. Consult [ROADMAP.md](ROADMAP.md) and [TODO.md](TODO.md), plus relevant GitHub Issues and pull requests, before planning work.
6. Check [CHANGELOG.md](CHANGELOG.md), the actual target branch, source files, tests and merged history. Documentation alone does not demonstrate implementation.

## Authority and change control

- **WHAT:** SPEC owns requirements, product scope, business rules and educational objectives. Only the Product Owner can approve substantive scope changes.
- **OBSERVABLE BEHAVIOR:** FEATURES inventories testable gameplay and must remain consistent with SPEC.
- **HOW:** ARCHITECTURE owns structure, state authority, runtime ordering and dependency constraints.
- **WHY:** [Architecture decision log](docs/architecture/DECISIONS.md) tracks rationale and unresolved choices; no decision record silently overrides SPEC or ARCHITECTURE.
- **WHEN:** ROADMAP owns milestones M00–M09. TODO owns the current documented task list; GitHub Issues take priority as the live execution tracker once established.
- **EVIDENCE:** Tests, code, CI and deployment records establish implementation, verification and release status. CHANGELOG documents only integrated, evidenced changes.

If sources conflict, identify the subject-matter authority and impacted behavior. Report the discrepancy instead of silently changing a requirement or weakening a validation rule.

## Non-negotiable project constraints

- Build toward one complete Release 1.0 from T0; milestones are integration gates, not separate MVPs or promises of partial public releases.
- Default city: **160 × 120 tiles** with coherent roads, districts, zones, lots and accessible establishments.
- Standard scenario: **50 initial AI companies**, at least **40 feasible, independently active AI trading counterparties** at the initial and each completed-day boundary, including 15 supplier and 15 B2B customer options; **60+ SKUs** across **10+ sectors**.
- One authoritative commercial order, shown as buyer PO and seller SO; never invent an independent second settlement. The same rules apply to human and AI actors.
- Preserve the eleven ordered daily phases in SPEC §11, deterministic PRNG streams, canonical state, stable IDs and reproducible replay.
- Keep the strict TypeScript simulation independent from React, DOM, Canvas, IndexedDB and Vercel; UI submits typed commands and consumes read-only projections.
- User-facing gameplay and learning explanations default to **pt-BR**; technical documentation, source identifiers and code use English.
- No required student identity, paid service, generative AI, backend or cloud persistence for Release 1.0. Validate imported files as untrusted content.
- Do not replace economic behavior with cosmetic counters, mock firms, infinite money/stock, or nondeterministic shortcuts.

All exact rules and exceptions are defined in SPEC and ARCHITECTURE, not duplicated here.

## Execution and review protocol

1. Identify the associated SPEC section, FEATURES capability (existing Fxx identifier), TODO task (Mxx-Tyy), target branch and dependencies.
2. Inspect existing code and tests; do not assume that a planned component exists. Check current GitHub Issues and recent merged changes if accessible.
3. Assess changes to invariants, order/ledger ownership, module dependencies, replay, save compatibility, UI, accessibility, localization and education.
4. Implement a coherent, reviewable change. Keep unrelated files intact; use small PRs and avoid resetting, force-pushing or bypassing branch rules.
5. Provide relevant verification: typecheck, lint, unit/integration/scenario tests, deterministic fixtures, build, browser E2E and acceptance evidence as applicable.
6. Update only documents whose owned contract changed. Do not copy detailed requirements or task queues into other documents.
7. Report the exact files/commit/PR, test results, remaining gaps, and any decision requiring Product Owner approval.

### Local commands

The repository currently has documentation only, not an application scaffold. Do not claim that npm commands can run until package.json and scripts are committed. Planned commands (to verify after M00-T06/T07): dev, build, typecheck, lint, test, and test:e2e. Never invent successful CI or deployment results.

## Specialized reference documents

- [Testing and verification](docs/quality/TESTING.md): evidence and gates.
- [Conceptual data model](docs/architecture/DATA_MODEL.md): entities, ownership and persistence.
- [Internal interface contracts](docs/architecture/INTERFACES.md): command, event, projection and save boundaries.
- [Architecture decision log](docs/architecture/DECISIONS.md): approved rationale and pending decisions.
- [Experience and interface design](docs/design/DESIGN.md): navigation, interactions and accessibility.
- [Operational deployment](docs/operations/OPERATIONS.md): planned Vercel publication and rollback.
- [Risk register](docs/operations/RISKS.md): tracked delivery/technical exposures.
- [Domain glossary](docs/GLOSSARY.md): terminology.
- [Security policy](SECURITY.md): vulnerabilities, trust boundaries and privacy.

## Documentation update triggers

- Requirement change: SPEC → affected FEATURES, ARCHITECTURE and ROADMAP.
- Structural/interface decision: ARCHITECTURE and, if significant, a decision record → affected interfaces/tests.
- Implementation or fix: code + tests → affected behavior, planning and evidence records.
- Integrated change: evaluate README and CHANGELOG; do not call planned work released.
- Release: update CHANGELOG only after a verified version, artifact or deployment exists.
