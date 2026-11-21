# Domain Glossary — Supply Chain Simulator

**Status:** Working vocabulary derived from current product requirements. [SPEC.md](../SPEC.md) defines the rules; this glossary explains terms rather than overriding them.

## Vocabulary convention

Source code and technical documentation use English; the player UI uses pt-BR. Keep stable source identifiers separate from translated display labels. Use the same definitions in tutorials, tooltips, orders, reports and instructor materials.

| Term | Meaning in this game | Suggested pt-BR |
| --- | --- | --- |
| Company | Player- or AI-controlled legal/economic operator owning facilities and assets | Empresa |
| Facility / Establishment | Operating store, factory, farm or warehouse belonging to one company and located on a lot | Estabelecimento |
| Lot | Contiguous, zone-classified, independently owned tile footprint with road access | Lote |
| Zone / Zoning | Construction compatibility of a lot: industrial, agricultural, residential or commercial | Zona / Zoneamento |
| SKU / Product | Distinct tradable catalog item with a unit and market/recipe relationships | Produto / Código de produto |
| Supplier offer | Seller's available product, pricing and commercial conditions | Oferta do fornecedor |
| Procurement request | Named AI customer's published need with price/quantity/validity constraints | Solicitação de compra |
| Purchase Order (PO) | Buying party's view of the single canonical B2B commercial order | Pedido de Compra (PC) |
| Sales Order (SO) | Selling party's view of that **same** B2B order; not an independent settlement | Pedido de Venda (PV) |
| Sales quote | Player/seller proposal to fulfill a buyer's procurement request | Cotação de venda |
| Lead time | Days between the relevant order/dispatch boundary and expected receipt under the defined logistics rules | Prazo de entrega |
| Landed cost | Purchase amount plus attributable inbound freight/cost basis at receipt | Custo posto no destino |
| On-hand inventory | Physically available inventory at a given facility, before commitments are deducted | Estoque físico |
| Reserved outbound | Stock committed to accepted outgoing orders, not freely available for new orders | Estoque reservado |
| Available-to-spend cash | Funds not already committed to confirmed purchasing/freight obligations | Caixa disponível |
| Shipment / Goods receipt | Dispatched cargo in transit / validated receipt into destination stock | Remessa / Recebimento |
| Cost of goods sold (COGS) | Recognized cost basis of goods actually sold, not every cash purchase | Custo das mercadorias vendidas (CMV) |
| Net profit | Simplified recognized revenue minus recognized COGS and operating expenses, distinct from cash movement | Lucro líquido |
| Return on investment (ROI) | Defined return ratio using cumulative profit and contributed equity, not a synonym for revenue | Retorno sobre investimento |
| Seed | Reproducible initial value used with scenario versions and ordered actions to generate equivalent outcomes | Semente |
| Counterparty | Independently operated, active and feasible supplier and/or B2B buyer selectable for trade | Parceiro comercial |
| Market ecology | Funded entry, growth, distress, liquidation and closure of companies under deterministic rules | Dinâmica empresarial |

## Ambiguities to avoid

- A **company** is not its **facility** and neither is the **lot**.
- A PO plus an SO is **one** commercial transaction, not two invoices or sales.
- A stock purchase changes cash/assets; it does not immediately imply COGS.
- **Cash**, **revenue**, **profit** and **ROI** are different measures.
- The requirement for **40 trade-accessible counterparties** counts viable, distinct AI companies, not decorative map icons or 40 completed trades.
- A **milestone** is an integration gate; it is not a separately released MVP.

Refer to [FEATURES.md](../FEATURES.md) for observable examples, [ARCHITECTURE.md](../ARCHITECTURE.md) for model ownership and [DESIGN.md](design/DESIGN.md) for in-game terminology use. Domain-owner approval is required before changing a term's economic meaning.
