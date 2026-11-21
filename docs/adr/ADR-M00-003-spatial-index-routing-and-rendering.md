# ADR-M00-003 — World Graph, Route Index and Renderer Chunks

**Status:** Proposed — measurement-dependent / not accepted. **Owner:** Software Architect. **Approver:** Product Owner pending. **Issue/Story:** #3 / TL-004.

## Fixed boundaries
[SPEC §4](../../SPEC.md), [ARCHITECTURE §§6,9,16](../../ARCHITECTURE.md): default 160×120 tile seeded city, coherent zone/road/lot access, world domain owns geometry, logistics calculates routes and freight, renderer receives read-only visual projections. Canvas sprites/chunks, caches and viewport positions cannot become authoritative economic state. No benchmark or running game exists.

## Candidate implementations
- **Road topology:** A — packed integer tile graph / adjacency arrays with stable tile-index scheme; B — sparse directed road-edge table keyed by deterministic IDs. Evaluate memory and mutation/rebuild complexity against actual lot/road generation; no selected data representation.
- **Routing:** A — deterministic Dijkstra with stable neighbor/edge tie-breaks; B — deterministic A* with admissible heuristics and stable tie-breaks. Both must produce deterministic chosen route when multiple paths have equal cost; no speculative speed claims.
- **Route cache:** A — no persistent cache, calculate on demand; B — rebuildable LRU keyed by world graph revision, endpoints and routing algorithm version. Cached results MUST be bit-equivalent to uncached, invalidated by changed roads, and excluded from canonical hash.
- **Renderer chunks:** A — fixed-size spatial chunks with viewport culling; B — row/region buckets generated from the world projection. Candidate chunk sizes are experiment parameters, not architectural decisions.

## Reproducible measurement protocol
Build fixtures for at least: dense 160×120 road grid; sparse disconnected roads; districts with narrow bottlenecks; fully occupied legal lots; rapid camera movement; many facilities/truck routes; repeated path requests before/after road changes. Fix seed, catalog and simulation revision. Record machine/OS/browser versions, renderer scale, route sample count, measured wall times (median/tail), memory, cache-hit rate, and deterministic route hashes. Use the same fixtures for all alternatives. Do not invent thresholds, timings or pass values. Include negative cases: unreachable destinations, inconsistent lot-road access, stale cache, equal-cost route ties, viewport clipping, invalid coordinate.

## Consequence and decision gate
Packed grids may be compact for a fixed city but have less adaptable graph mutation; sparse edges can express road semantics more flexibly at allocation cost. Cached routing trades memory and invalidation complexity for potential repeated-query speed; chunk strategies trade update work against draw culling. **No option is chosen without measurements and review**; the 160×120 requirement remains fixed.
