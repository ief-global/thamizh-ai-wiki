# Packaging & deployment — from your laptop to a public service

> The release ladder: each rung independently useful, each gated by the
> [license audit](05-sources-provenance.md#licensing-stance-public-release-gate).

## Rung 0 — run it locally (developers, today)

The server speaks MCP over **stdio**: your AI client launches it as a subprocess. Install foma
(`apt install foma`), then point your MCP client (Claude Desktop / Claude Code / Cursor…) at:

```jsonc
{ "mcpServers": { "thamizh": {
    "command": "uvx",
    "args": ["--from", "git+https://github.com/ief-global/thamizh-mcp", "thamizh-mcp"] } } }
```

Ask your assistant "மரத்தில் — இலக்கணக் குறிப்பு?" and watch it call the tools.

## Rung 1 — PyPI + Docker

- **PyPI**: `uvx thamizh-mcp` with no git URL; the standard MCP install path.
- **Docker (GHCR)**: the image bundles foma + FSTs + code — the zero-friction path for anyone who
  doesn't want to manage a C binary. Same stdio protocol, `docker run` as the command.

## Rung 2 — a hosted instance (reaching non-developers)

Most Tamil speakers will never install an MCP client. For them:

- The engine gains a **streamable-HTTP** MCP endpoint and a small **REST API**
  (`GET /analyze?word=மரத்தில்` → the WordAnalysis JSON) — same engine, two doors.
- One container on a scale-to-zero cloud runtime, behind an edge cache (repeated word lookups are
  ideal cache candidates) and rate limiting.
- A simple **web page**: type a word, see the sourced analysis rendered in Tamil — plus an
  embeddable widget other Tamil sites can drop in, and later a browser extension
  (highlight a word → analysis popup).
- Operated **as a non-profit public service under the `ief-global` umbrella** — free to use, and the
  Docker image means any university or community can self-host their own instance independently.

The hosted instance has a second job: its knowledge store sees all its users' queries, making it the
**central gold-data accumulator** for the [curation pipeline](../03-llm-layer/03-data-curation.md).

## Rung 3 — discoverability

Listings on the MCP registries (the official registry + community directories) and — most valuable
for this niche — the **Tamil-NLP community catalogs**, so Tamil developers find it where they already
look. A Hugging Face **Space** demo comes with the hosted instance
([HF plan](../03-llm-layer/04-huggingface-plan.md)).

## Versioning & integrity

Releases are tagged from a protected main branch; the output schema is versioned; anchor-data pins
(FST commit, dataset commits) ship in `data/PINS.md` so any answer can be reproduced against the exact
data that produced it.

→ Next section: [What is a GPT?](../03-llm-layer/01-what-is-a-gpt.md)
