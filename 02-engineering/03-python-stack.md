# Python stack, project layout, and testing

## Why Python

The entire Tamil-NLP ecosystem we ground against is Python or native binary: ThamizhiMorph rides on
**foma** (native C, called via `flookup`), and **open-tamil**, ThamizhiPOSt/LIP, and Stanza are Python
packages. Putting the analysis engine and the MCP server in one Python process means direct library
calls, one language, no inter-process boundary. (A TypeScript server calling a Python service was
considered and rejected — an IPC hop for zero linguistic benefit.)

## The layers

| Layer | Choice | Role |
|---|---|---|
| MCP framework | **FastMCP** | registers tools, handles the MCP protocol over stdio (HTTP later) |
| Language | Python 3.10+ | engine + adapters + decoders |
| Packaging | **uv** (`pyproject.toml` + `uv.lock`) | reproducible installs; users run via `uvx thamizh-mcp` |
| Morphology | **foma / flookup** (system dependency) + pinned ThamizhiMorph FSTs | analysis & generation |
| Text utils | open-tamil | NFC normalization, Tamil-script validation, Grantha detection |
| HTTP | httpx (async) | evolving-source pulls, with timeouts + descriptive User-Agent |
| Store | SQLite (stdlib) | claims cache + transaction log |
| Validation | Pydantic | the WordAnalysis schema — the single output contract |

**uv in one paragraph:** a fast Python package manager that locks exact dependency versions
(`uv.lock`) so every machine builds the identical environment, and whose `uvx` runner lets an end
user launch the server with one line — no clone, no virtualenv ceremony. MCP clients (Claude Desktop,
Cursor…) are configured to run exactly that command.

**The one awkward dependency:** foma is a C binary, not pip-installable. Local users install it from
their OS packages; everyone else gets the **Docker image with foma baked in** — see
[Deployment](06-deployment.md). If `flookup` is missing, the server fails with an actionable message
instead of degrading silently.

## Project layout (the public repo)

```
thamizh-mcp/
  src/thamizh_mcp/
    server.py          # FastMCP head — thin; tools call the engine
    core/engine.py     # orchestration: cache → anchors → evolving → merge
    core/decoder.py    # FST tags → பகுபத உறுப்பு / வேற்றுமை (+ authority)
    core/classifier.py # origin classification rules
    adapters/          # thamizhimorph · wiktionary · equivalents · lexicon (one interface)
    store/             # SQLite knowledge store (claims + transactions)
    schema.py          # Pydantic WordAnalysis contract
  data/                # pinned FSTs, equivalents CSVs, PINS.md (versions + citations)
  tests/               # pytest suite + fixture words
```

## How it's tested

- **87 pytest tests** (85 runnable without foma — FST-dependent tests skip cleanly when `flookup`
  is absent, so contributors without foma still get a green suite).
- **Network is mocked** in unit tests: the Wiktionary adapter is exercised against a captured real
  entry (புத்தகம்'s actual wikitext) so the parser is tested against reality without live calls.
- **Behavioural guarantees are tested, not just functions:** cache write-back on enrichment, honest
  gap on source timeout, gap notes carrying *which* source failed, rejection of non-Tamil input,
  preservation of multiple analyses.
- **Fixture words** cover each concern: மரம் (simple native), மரத்தில் (sandhi + case ambiguity),
  புத்தகம் (வடசொல் with equivalent), கம்ப்யூட்டர் (English loan), ஜன்னல் (Portuguese loan), plus
  no-equivalent and disputed-origin words for the honesty paths.
- Server-level regression evals are separate from the **product-level A/B benchmark**
  ([morphological lift](../03-llm-layer/02-mcp-meets-llm.md)) — passing tests prove the tools are
  correct; the lift benchmark proves they matter.

## Workflow

Development on a `develop` branch → PR → squash-merge to protected `main`. Anchor data never changes
without its pin + citation updating in `data/PINS.md`.

→ Next: [The MCP tools](04-mcp-tools.md) · [Deployment](06-deployment.md)
