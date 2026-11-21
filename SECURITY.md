# Security Policy — Supply Chain Simulator

**Status:** Draft, pre-implementation. No release, public vulnerability-reporting channel or security testing program is yet verified.

## Scope and supported versions

The planned Release 1.0 is a browser-only, static application using local IndexedDB and validated JSON/CSV import/export. There is no released/supported software version yet and no planned required account, backend service or student identity collection.

Security requirements originate in [SPEC §15](SPEC.md#15-security-privacy-and-accessibility); implementation boundaries are defined in [ARCHITECTURE §18](ARCHITECTURE.md#18-security-accessibility-and-operational-boundaries).

## Reporting vulnerabilities

**Approved private reporting contact:** {{VULNERABILITY_REPORTING_CHANNEL}} (Product Owner must supply or enable a repository security advisory route).

Until verified, **do not assume that ordinary public Issues provide confidential reporting**. Avoid publicly disclosing exploit details, student data or credentials. The Product Owner must establish triage ownership, acknowledgment expectations and a coordinated disclosure policy before the application is published.

## Relevant trust boundaries and controls

- **Imported content is untrusted:** strictly bound JSON/CSV file size, parsed types, numeric ranges, entity references, schema versions and displayed strings; no eval, dynamic script execution or trusted arbitrary formulas.
- **Local storage:** load into validated, consistent state; handle corruption and incompatible versions with explicit errors instead of partially applied imports.
- **No mandatory personal data:** do not require names, emails or logins; reports should use scenario identifiers or optional aliases, not school records.
- **No embedded secrets:** static client assets and Git history must not contain API keys, access tokens or private school records.
- **Dependencies and assets:** track source licenses, review package dependencies, avoid unnecessary runtime remote services, and verify asset redistribution rights.
- **UI:** escape or safely render user-imported names and text, including report displays; maintain keyboard-accessible error handling.
- **Deployments:** never mistake the planned Vercel configuration for a verified secure live environment.

## Planned verification

Create hostile input fixtures, oversized/corrupted import tests, schema rejection tests, script-injection rendering tests, dependency review and static build checks as part of [Testing](docs/quality/TESTING.md). Link actual security evidence after code and CI exist.

**Open governance:** approved disclosure contact, triage owner, dependency scanning configuration and a release-specific security review are not yet established.
