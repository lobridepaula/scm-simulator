# Product Experience and Interface Design — Supply Chain Simulator

**Status:** Planned UI/UX contract. No screens, visual tokens, assets or running frontend have been verified.

## Audience and design principles

Target: learners in the Brazilian Assistente Administrativo de Projetos course, with a teacher assigning comparable 30-day scenarios. The interface must make economic causes and consequences inspectable without requiring ERP experience.

- Default gameplay language: **pt-BR**. Source identifiers and technical specifications remain in English.
- Accessibility is functional: full keyboard paths, visible focus, readable contrast, labels, status descriptions beyond color, no audio dependency and reduced motion where relevant.
- Optimize for classroom desktop/laptop use; tablet usability is desirable.
- The same action/feedback semantics must apply to player↔AI purchasing and customer quotation, and to ordinary accepted/rejected paths.

## Primary navigation

1. **City / World:** 160 × 120 tile map, districts/roads/lots, zoning, ownership, accessibility/competition overlays and lot details.
2. **Company dashboard:** cash, recognized revenue, simplified net profit, ROI, trend, alerts and active facilities.
3. **Market / directory:** at least 40 independently accessible AI companies; filter supplier vs customer, products, offers, customer requests, price, distance, freight and lead time.
4. **Orders and logistics:** Purchases, Sales, Transfers and Shipments; status, counterparties, reservations, fulfillment and explanations.
5. **Facilities and inventory:** capacities, production recipes, stock, pricing, outgoing offers and operational restrictions.
6. **Learning / missions:** objectives, relevant concepts, decisions made, result explanations and local report export.

These areas are specified in [SPEC §12](../../SPEC.md#12-main-user-interface). This document owns interaction conventions, not gameplay requirements.

## Required user journeys

**Introductory purchase:** choose company/product → inspect two feasible suppliers → compare delivered cost and lead time → submit PO → see confirmation or rejection → advance days → inspect dispatch and receipt → reconcile cash, stock and profit.

**Seller-side B2B trade:** filter AI customer requests → select a named eligible customer → submit quote → inspect accepted/rejected outcome → track one canonical B2B order and fulfillment; never show fictitious guaranteed customer sales.

**Business expansion:** inspect eligible lot and zoning/access → purchase land → select lawful facility type → verify capitalized asset, operating cost and inventory/capacity effects.

**Classroom review:** inspect seed/day, objectives, purchase choices, operational metrics and final 30-day report → export locally using the scenario identity rather than mandatory student data.

## Visual and interaction patterns

- **Map:** chunked viewport-culled Canvas 2D; pan, zoom, selected-lot highlight and readable overlay legends. Use list and directory navigation as equivalent non-map access.
- **Color meaning:** industrial red, agricultural yellow, residential green and commercial blue per SPEC, always reinforced by text/icon/pattern. No purely color-dependent decisions.
- **Order state:** explicitly label buyer PO versus seller SO; reveal pending/reserved/transit/delivered status and concise rejection reasons.
- **Money:** format Brazilian real currency for players; distinguish cash flows, profit and capital investment with explanations, not just differently colored KPIs.
- **Feedback:** confirmations identify transaction and effect; failures identify actionable constraint and do not imply resources were moved; empty directories explain filters/feasibility.
- **Assets:** use a versioned, licensed local sprite manifest and graceful fallback; unverified candidate art is not approved for bundling.
- **Theme, typography, breakpoints and layout tokens:** {{APPROVED_VISUAL_TOKENS_AND_BREAKPOINTS}}. Do not freeze aesthetics before reviewing prototypes and classroom devices.

## Validation and governance

Usability tests must include keyboard-only first purchase, search without map interaction, one rejection path, a 30-day teacher review, localization checks and distinct explanation of revenue/profit/cash/ROI.

Behavior acceptance belongs to [FEATURES.md](../../FEATURES.md), technology to [ARCHITECTURE.md](../../ARCHITECTURE.md), quality gates to [Testing](../quality/TESTING.md), and pedagogy to [SPEC.md](../../SPEC.md).
