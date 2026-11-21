# Operations and Deployment — Supply Chain Simulator

**Status:** Pre-implementation runbook. Neither a live deployment nor operational commands are verified.

## Target topology

- **Source:** [GitHub repository](https://github.com/lobridepaula/scm-simulator), branch main.
- **Planned build:** strict TypeScript + React/Vite static site. The repository currently has no application scaffold or verified package scripts.
- **Planned hosting:** Vercel static deployment linked to GitHub; no required backend, service database, paid API or account.
- **Runtime state:** browser memory and IndexedDB with locally exported JSON/CSV; the hosting platform does not hold game progress.

See [ARCHITECTURE.md](../../ARCHITECTURE.md) for the authoritative technical runtime and [SPEC.md](../../SPEC.md) for required offline/local behavior.

## Deployment readiness checklist

1. Confirm application scaffold, dependency lockfile, build output and scripts from actual repository files.
2. Confirm branch policy, Vercel project linkage and environment/production configuration. **No values are assumed here.**
3. Run CI typecheck, lint, domain/integration/scenario tests, browser acceptance tests, accessibility checks and static production build.
4. Validate bundled asset provenance/licensing and absence of client-embedded secrets.
5. Benchmark the 160 × 120 / 50-firm / 40-counterparty scenario on representative classroom machines.
6. Publish a traceable production artifact and smoke-test: app load, scenario generation, order and shipment flow, local save, report export and safe reload.
7. Document the verified deployment URL, source SHA, operator, date, monitoring and rollback path before calling it released.

## Rollback and recovery

**Deployment rollback:** {{VERIFIED_VERCEL_ROLLBACK_PROCEDURE}}. Document only after confirming the available Vercel project and immutable deployment history.

**Game-state continuity:** the product has no server backup. Recovery depends on correct local saves and user-controlled JSON exports. Test version compatibility/migration, imported save validation, interruption behavior and clear user instructions; never promise recovery of a deleted browser profile.

## Observability and incidents

- No production monitoring, alert thresholds, on-call owner, support address or SLO is currently configured.
- Planned evidence: CI outcomes, deployed SHA, static asset integrity, browser errors and manual classroom smoke-test results, without uploading confidential student identity or save data.
- Incident protocol to finalize: {{OPERATOR_AND_ESCALATION_CONTACT}}, {{INCIDENT_RESPONSE_AND_ROLLBACK_OWNER}}.

## Maintenance

Review dependencies and asset licenses; validate save compatibility before schema changes; re-run deterministic city/economy fixtures and the classroom end-to-end route for each release candidate.

Record completed changes in [CHANGELOG.md](../../CHANGELOG.md); capture planned work in [TODO.md](../../TODO.md), not here.
