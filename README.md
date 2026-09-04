# FACTANKER — MCP Server

**The independent proof layer for AI answers.** When an AI states a number,
FACTANKER is the evidence behind it: every figure traces to an official
primary record, from SEC filings to FFIEC call reports, IRS 990s and
USAspending awards. FACTANKER does not fact-check news or opinions; it
anchors numbers in filings.

**1.18B+ evidence-backed facts** about US companies, banks, nonprofits and
federal spending. Every value carries its evidence: filing reference
(e.g. SEC accession number), retrieval timestamp, license and a stable
citation URL. Read-only, no API key, no auth, no signup.

- **MCP endpoint:** `https://factanker.com/mcp` (streamable HTTP)
- **Docs / quickstart:** https://factanker.com/quickstart
- **Machine catalog:** https://factanker.com/llms.txt · https://factanker.com/llms-full.txt
- **Terms:** https://factanker.com/nutzung

## Connect (Claude, ChatGPT, any MCP client)

```json
{
  "mcpServers": {
    "factanker": {
      "type": "streamable-http",
      "url": "https://factanker.com/mcp"
    }
  }
}
```

## Tools

| Tool | What it does |
|---|---|
| `query_template` | Curated queries: company financials (SEC), bank metrics (FFIEC), nonprofit finances (IRS 990), peer percentiles, federal-contract dependency. Answers include the executed query, result count, scope and an explicit `not_claimed` section. |
| `list_templates` | Self-description of all templates: parameters, required fields, allowed enum values. |
| `lookup_entity` | Resolve a company, bank or nonprofit to all its official identifiers (CIK, LEI, EIN, UEI, RSSD, FDIC cert, NPI, QID). |
| `get_facts` | All current evidence-backed facts for one entity, each with source, filing reference and citable `fact_url`. |
| `search_facts` | Full-text search across entities and predicates. |
| `mcp_server_history` | Claim history of remote MCP servers from FACTANKER's own periodic probes. |

## REST (same data, plain URLs)

```
curl 'https://factanker.com/api/query/company_financials?cik=320193&metric=revenue&year_from=2023'
curl 'https://factanker.com/api/lookup_entity?q=cik:0000936468'
```

Every fact URL serves JSON under `Accept: application/json` (same URL as HTML).

## Citation

Please cite answers with their `fact_url` so readers can inspect the
evidence: `Cite as: FACTANKER, https://factanker.com/fact/<id>`

## Operator

Enerlio GmbH · kontakt@enerlio.de · https://factanker.com/impressum

The server code is operated privately; this repository is the public
description and quickstart for the MCP endpoint.
