# FACTANKER — MCP Server

**The independent proof layer for AI answers.** When an AI states a number,
FACTANKER is the evidence behind it: every figure traces to an official
primary record, from SEC filings to FFIEC call reports, IRS 990s and
USAspending awards. FACTANKER does not fact-check news or opinions; it
anchors numbers in filings.

**1.65B+ evidence-backed facts · 80M+ entities · 67 official sources** (live
counts at https://factanker.com/llms.txt). Coverage: US companies (SEC XBRL),
banks (FFIEC call reports), nonprofits (IRS 990), federal awards
(USAspending), corporate structure (GLEIF, SAM), power plants (EIA), US
regulations (CFR) — plus interest rates and FX (Federal Reserve, ECB),
Eurostat indicators, German renewables by region (Marktstammdatenregister),
German power prices (SMARD), German job postings and German federal law
(BImSchG, EEG, EnWG). Every value carries its evidence: filing or record
reference, retrieval timestamp, license and a stable citation URL.
Read-only, no API key, no auth, no signup.

**License per fact.** US federal records are public domain; German and EU
sources (e.g. DL-DE-BY-2.0, CC BY 4.0, ECB/Eurostat reuse terms) require
naming the source — each fact states its license and, where needed, the
attribution text to use.

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
| `query_template` | Curated queries: company financials (SEC), bank metrics (FFIEC), nonprofit finances (IRS 990), peer percentiles, federal-contract dependency, German renewables by state/district/municipality (`mastr_region`). Answers include the executed query, result count, scope and an explicit `not_claimed` section. |
| `list_templates` | Self-description of all templates: parameters, required fields, allowed enum values. |
| `lookup_entity` | Resolve a company, bank or nonprofit to all its official identifiers (CIK, LEI, EIN, UEI, RSSD, FDIC cert, NPI, QID). |
| `get_facts` | All current evidence-backed facts for one entity, each with source, filing reference and citable `fact_url`. |
| `search_facts` | Free-text search ("Kroger net income", "fed funds rate"): resolves metric and entity, returns the latest matching facts with provenance. |
| `get_timeseries` | One metric over time for one entity; annual values kept apart from quarters, each point with period start/end. |
| `verify_claim` | Check a stated number against the official record and return the evidence. |
| `compare_entities` | Same metric across several entities, aligned by period. |
| `describe_predicate` | What a metric means: unit, value fields, sources, coverage. |
| `get_source` | Publisher, license, coverage and a ready-made citation for a source. |
| `mcp_server_history` | Claim history of remote MCP servers from FACTANKER's own periodic probes. |

## Skills

Ready-made agent skills and a Claude Code plugin (verify figures, peer
benchmarks, rates & FX, hiring checks): https://github.com/Askrion/factanker-skills

## REST (same data, plain URLs) — large calls, no key

One request returns a whole dataset:

```
# Every fact on record for Apple — 500 facts, each with its source document
curl 'https://factanker.com/api/get_facts?entity=cik:0000320193'

# Apple revenue for every reported quarter and year
curl 'https://factanker.com/api/query/company_financials?cik=320193&metric=revenue&period=all'

# Net income of Apple, Microsoft, Amazon, Alphabet, Nvidia and Meta since 2015
curl 'https://factanker.com/api/query/metric_for_tickers?ciks=320193,789019,1018724,1652044,1045810,1326801&metric=net_income&year_from=2015'

# JPMorgan Chase Bank total assets, quarter by quarter (FFIEC Call Reports)
curl 'https://factanker.com/api/query/bank_metrics?rssd=852218&metric=total_assets'

# One period only: 2025, 2025Q4 or 2025-12-31
curl 'https://factanker.com/api/get_facts?entity=rssd:852218&predicate=total_assets&period=2025Q4'

# German wind power by district (Marktstammdatenregister)
curl 'https://factanker.com/api/query/mastr_region?level=landkreis&energy_source=wind'

# Free-text search
curl 'https://factanker.com/api/search_facts?q=Kroger%20net%20income'
```

All endpoints with tested examples: https://factanker.com/api/docs ·
endpoint list: https://factanker.com/api/ · OpenAPI: https://factanker.com/api/openapi.json

A call with a missing or wrong parameter answers with the correct usage and
working example URLs. MCP tool names (`get_timeseries`, `compare_entities`, ...)
also work as REST paths. Every fact URL serves JSON under
`Accept: application/json` (same URL as HTML).

## Citation

Please cite answers with their `fact_url` so readers can inspect the
evidence: `Cite as: FACTANKER, https://factanker.com/fact/<id>`

## Operator

Enerlio GmbH · kontakt@enerlio.de · https://factanker.com/impressum

The server code is operated privately; this repository is the public
description and quickstart for the MCP endpoint.
