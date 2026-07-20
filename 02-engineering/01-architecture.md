# Architecture — one engine, thin heads, a self-enriching store

> How a Tamil word travels through the system, and why the design is shaped this way.
> Non-technical background: [Our approach](../01-origin/04-our-approach.md).

## The data flow

```
input word (e.g. மரத்தில்)
  → normalize            Unicode NFC + Tamil-script validation (open-tamil); reject non-Tamil cleanly
  → knowledge store      SQLite cache — hit? serve with stored provenance
  → on miss: adapters    each source wrapped behind one uniform interface
       anchors:  ThamizhiMorph FST (flookup) · Tholkappiyam rule table · pinned lexicons/glossaries
       evolving: Tamil Wiktionary · Indic-To-Pure-Tamil lists · (Aalamaram treebank, planned)
  → analysis core        FST-tag → பகுபத உறுப்பு decoder · case → வேற்றுமை mapper
                         origin classifier · attestation filter · authority tagger
  → merge + write back   provenance-tagged claims stored; store grows with use
  → heads                MCP tools (v1, live) | REST API (planned) | CLI (dev utility)
```

## Design rules that shape everything

- **One plain-Python engine, thin heads.** All logic lives in a library (`analyze_word(word) →
  WordAnalysis`); the MCP server, future REST API, and CLI are thin adapters over it. This is why a
  web tool and mobile backend come cheap later.
- **Two source tiers.** *Anchors* are version-pinned authorities (reproducibility = part of
  authenticity). *Evolving* sources are queried at need, cached with **source + tier + retrieval
  date** per claim. Details: [Sources & provenance](05-sources-provenance.md).
- **Self-enriching, never hand-maintained.** Word *forms* come from FST rules (no per-word upkeep);
  lexical knowledge accumulates from evolving pulls. Nobody babysits a word list.
- **Honest gaps are a feature.** When no source grounds a field, the tool returns an explicit gap with
  *which* source was missing. The merge layer drops any equivalent candidate lacking an attestation
  source — the LLM can never launder its own guess into a grounded claim.
- **All ambiguity surfaces.** மரத்தில் legitimately parses as locative *or* ablative; both readings
  return, each with provenance. Disambiguation belongs to a later, context-aware layer.

## The knowledge store

SQLite, two tables that matter:

- **`claims`** — one row per resolved fact per word: field, value, source, tier, retrieval date. The
  cache that makes repeat lookups instant and the audit trail behind every answer.
- **`transactions`** — one row per completed analysis (the full result + tool label + an
  `eval_fixture` flag). Not telemetry: this is the **gold-data flywheel** — the raw material the
  [data-curation pipeline](../03-llm-layer/03-data-curation.md) refines and publishes. Benchmark
  words are flagged so they can never leak into published training data.

Writes are serialized (SQLite single-writer discipline); the FST itself stays stateless.

## Non-blocking by construction

The heavy work is blocking: `flookup` is a subprocess; evolving pulls are network I/O. Handlers push
both off the event loop (`anyio.to_thread` for subprocess/sync calls, async HTTP client for pulls),
bound concurrency, and put a **timeout on every external call** — a timed-out source returns an honest
gap rather than hanging the request.

→ Next: [FST & ThamizhiMorph](02-fst-thamizhimorph.md) · [The MCP tools](04-mcp-tools.md) ·
[Python stack & testing](03-python-stack.md)
