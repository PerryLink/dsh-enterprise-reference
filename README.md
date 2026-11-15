# Enterprise DSH Reference Architecture

> A vendor-neutral reference for running DeepSeek Harness (DSH) inside an organization: identity, permissions, audit, MCP integration, and offline upgrade/rollback.
> **Independent community project. Not affiliated with DeepSeek.** License: CC-BY-4.0.

DSH is a fast-moving open-source agent harness. It is excellent for individual developers, but enterprises ask different questions:

- Who can run what, on which data, with which tools?
- Can we prove what the agent did six months later?
- How do we connect internal systems without giving the agent the keys to everything?
- How do we upgrade a fast-moving preview without breaking 200 plugins?

This repository is a **reference architecture + 80-item checklist**, not a product. Use it to scope a pilot, write an RFP, or review a vendor.

## Contents

| File | Purpose |
|---|---|
| `WHITEPAPER.md` | architecture, four pillars, deployment models, adoption roadmap |
| `CHECKLIST.md` | 80 checks across 10 domains, with evidence column |
| `docs/REFERENCE-ARCHITECTURE.md` | mermaid diagram + component responsibilities |
| `docs/CASE-STUDY-TEMPLATE.md` | one-page template for a pilot write-up |
| `docs/MATURITY-MODEL.md` | L1–L4 maturity model |

## How to use

1. Run the checklist as a gap assessment; score each item 0/1/2.
2. Pick one deployment model (single team / team platform / air-gapped).
3. Do a 4-week pilot with one workflow, one data source, and one integration.
4. Write the case study with the template; do not publish customer data.

## Non-goals

- Not a security certification.
- Not a DeepSeek product or roadmap.
- Not legal advice.

## Contributing

Corrections and field experience are welcome. Please open an issue with the checklist item number and evidence.

## Related seeds

- [dsh-hcb](https://github.com/PerryLink/dsh-hcb) 0.10.2 — **143 checked / 3 FLOW `manual`**, checker **143/143 rows on `dsh-standards@1.7.0`**: check plugin compatibility before upgrades.
- [dsh-ecosystem-archive](https://github.com/PerryLink/dsh-ecosystem-archive) — dated ecosystem snapshots for procurement/trend context.
