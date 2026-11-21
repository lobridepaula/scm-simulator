# Supply Chain Simulator

**An educational business and supply-chain simulation for Brazilian vocational education.**

Supply Chain Simulator is a planned browser-based management game in which students direct a company in a procedurally generated city. Inspired by the economic decision-making of *Capitalism Lab*, the project focuses on learning **procurement, sales, inventory, production, logistics, and financial performance** through a simpler, classroom-oriented experience.

> **Project status: Specification / pre-implementation.** The game is not yet playable. The authoritative functional and technical requirements are documented in [SPEC.md](./SPEC.md).

## Educational purpose

The game is designed to support students in the Brazilian **Assistente Administrativo de Projetos** course. Players learn how decisions across a supply chain affect one another: choosing a supplier changes delivery time and cost, purchasing too much ties up cash, purchasing too little causes stockouts, and expansion creates both opportunities and fixed expenses.

Its teaching approach emphasizes **learning through decisions and their measurable consequences**, rather than memorizing definitions.

### BNCC alignment

The educational design proposes links to Brazil's **Base Nacional Comum Curricular (BNCC)**, especially general competencies related to critical thinking, digital culture, the world of work, evidence-based argumentation, and responsible decision-making.

Selected High School Mathematics references include **EM13MAT101** (interpreting changes in economic quantities) and **EM13MAT203** (using mathematics and digital tools in planning and decisions). **EM13MAT303** is reserved for a future financial-mathematics extension.

This is a **proposed pedagogical alignment**, not a declaration of formal BNCC compliance or professional-course accreditation. Activities must be adapted to the school's curriculum, students, and course modality. See [SPEC.md §13](./SPEC.md#13-pedagogical-design-and-bncc-mapping).

## How the game works

The player is the director of a company operating alongside autonomous, rule-based competitors. Companies can compete, buy from each other, and supply each other.

The core gameplay loop is:

1. **Plan** purchasing, inventory, production, and sales based on demand and available resources.
2. **Buy** products or inputs through traceable purchase orders.
3. **Receive** shipments, verify deliveries, and update inventory.
4. **Produce or distribute** goods through eligible establishments.
5. **Sell** to other companies or to the city's consumers.
6. **Evaluate** revenue, simplified net profit, cash, return on investment (ROI), and service performance.
7. **Expand** by acquiring lots and opening facilities where permitted.

### Orders first

**Every business-to-business trade between establishments is represented by a traceable order.** A transaction appears as a **Purchase Order (PO)** to the buyer and a **Sales Order (SO)** to the seller, with one underlying commercial transaction.

Orders move through confirmation, reservation, dispatch, transit, delivery, and receiving. Transfers between facilities belonging to the same company use internal transfer orders and **do not generate company revenue**.

### The city and its facilities

Each city is generated from a reproducible **seed** on a default **320 × 240 tile map (76,800 tiles)**. Major/local roads, districts and variable-sized lots shape location, routing, land value and market demand. Four zones govern construction:

| Zone | Map color | Allowed player facilities |
| --- | --- | --- |
| Industrial | Red | Factories, warehouses |
| Agricultural | Yellow | Farms, warehouses |
| Residential | Green | No player construction; generates consumer demand |
| Commercial | Blue | Stores, warehouses |

Facilities have distinct economic roles:

- **Stores** receive goods and sell to end consumers.
- **Factories** transform inputs into manufactured products.
- **Farms** harvest or extract primary resources.
- **Warehouses** store and dispatch products without manufacturing them.

Starter product chains include **fruit** (farm → store), **cheese** (dairy farm → factory → store), and **furniture** (resource producer → factory → store). They are teaching examples within a wider connected catalog of agricultural goods, industrial inputs and finished products.

### Competition and performance

The standard city supports **one player company and at least 40 trade-accessible AI companies** across a broad network of products, industries, suppliers, buyers and competing retailers. The player can choose individual counterparties for feasible B2B orders. AI firms follow reproducible business rules, not paid generative-AI services.

The business population is dynamic: companies can be founded, purchase property, open new units, lose competitiveness, become insolvent, liquidate assets and close. The standard scenario preserves a minimum of 40 accessible counterparties after each simulated day through a funded, validated entry pipeline. Closure preserves historical orders and property ownership records.

The headline business indicators are:

- **Revenue (Faturamento):** recognized external sales during the reporting period.
- **Simplified net profit (Lucro líquido):** recognized revenue minus cost of goods sold and recognized operating expenses.
- **ROI:** cumulative simplified net profit divided by cumulative contributed equity.

**Cash is tracked separately from profit.** Buying inventory or property can reduce cash without immediately reducing the period's operating profit—an intentional learning point.

## Classroom use

The intended introductory activity lasts **30 simulated days**. Students start with a company, choose a product, inspect suppliers, create a purchase order, receive goods, make sales, and explain their results. They can then expand by purchasing land and opening a permitted facility.

A planned **teacher mode** provides shared scenario seeds, learning objectives, and local result exports so students can be evaluated on both business outcomes and the decisions behind them. No student login is planned for the initial release.

The game interface is intended to use **Brazilian Portuguese (`pt-BR`)**, with technical documentation and source identifiers in **English**.

## Planned technology

| Layer | Planned approach |
| --- | --- |
| Language | TypeScript (strict mode) |
| Web interface | React + Vite |
| City rendering | Chunked, viewport-culled Canvas 2D with local, licensed visual assets |
| Simulation | Framework-independent, deterministic TypeScript engine |
| Competitors | 40+ rule-based AI trading counterparts with deterministic company entry, expansion and exit |
| Testing | Vitest; optional Playwright end-to-end tests |
| Local saves | IndexedDB and JSON import/export |
| Hosting | GitHub repository + static deployment on Vercel |
| Backend | None required for the initial release |

The same seed, scenario version, and ordered player actions must produce the same outcome. This supports reproducible tests and fair classroom comparisons.

### Development status and local setup

This repository currently contains the project specification and documentation, **not a runnable application**. Installation, development-server, test, and build commands will be added when the TypeScript/Vite project is scaffolded. Until then, there is no valid `npm install` / `npm run dev` workflow to follow here.

The live Vercel deployment URL will be linked once an application has been implemented and published.

## Implementation roadmap

The project targets **one complete Release 1.0**, with milestone gates **M00–M09** rather than separate reduced-scope releases. Implementation is incremental, but the 320 × 240 city, scalable market, 40+ viable AI company relationships, lifecycle rules, supply chains, financial integrity and complete educational experience are part of the product contract from T0.

See [ROADMAP.md](./ROADMAP.md) for milestone dependencies, [TODO.md](./TODO.md) for issue-ready tasks and [SPEC.md](./SPEC.md) for authoritative technical and gameplay requirements.

## Project documentation

- **[SPEC.md](./SPEC.md)** — Authoritative rules, spatial/market systems, educational goals, architecture and acceptance criteria.
- **[ROADMAP.md](./ROADMAP.md)** — Complete Release 1.0 milestone sequencing and verification gates.
- **[TODO.md](./TODO.md)** — Stable task identifiers, implementation requirements and acceptance checklist.
- **README.md** — Project overview and contributor entry point.

Before implementing or changing simulation behavior, review the relevant sections of `SPEC.md`. Keep business rules testable independently of the user interface, and avoid duplicating authoritative specifications in other documents.

## References

- [Base Nacional Comum Curricular — Ministério da Educação](https://basenacionalcomum.mec.gov.br/)
- [BNCC — Ensino Médio (MEC)](https://www.gov.br/mec/pt-br/cne/bncc_ensino_medio.pdf)
- [Brazilian Professional and Technological Education Guidelines — CNE/CP Resolution No. 1/2021](https://portal.mec.gov.br/dmdocuments/rcp001_21.pdf)

## License

A project license has **not yet been selected**. Public repository visibility does not, by itself, grant permission to reuse or redistribute its contents.
