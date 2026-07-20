# When MCP meets the LLM — grounding, and how we measure the lift

## MCP in one minute

**Model Context Protocol** is an open standard (2024) for connecting AI assistants to external tools —
one common socket instead of a custom plug per assistant. A tool server describes its tools ("I can
classify a Tamil word's origin; input: a word; output: this schema"); any MCP-capable assistant
(Claude, Cursor, and a growing list) can then *decide mid-conversation* to call them.

## The flow, concretely

```
User:  "புத்தகம்" இயற்சொல்லா? தூய தமிழ்ச் சொல் இருக்கிறதா?
LLM:   (recognizes a Tamil word-grammar question → calls tools instead of guessing)
        → classify_origin("புத்தகம்")      → வடசொல் (Sanskrit pustaka, தற்பவம்) [sources...]
        → suggest_native_equivalent(...)   → நூல் (literary), ஏடு — each with attestation
LLM:   composes the answer FROM the tool results, citing the sources —
        and can honestly say "no attested equivalent" when the tool says so.
```

The division of labor is the point: the **LLM** contributes language fluency, conversation, and
judgment about *when* to look things up; the **server** contributes ground truth, provenance, and
refusal-to-guess. Neither alone gives a trustworthy Tamil answer.

Because the server also returns clean roots and morpheme splits, it doubles as a
**de-agglutination layer**: downstream systems (semantic search / RAG pipelines) can index
வீடு+இற்கு+உள் instead of the shattered fragments of வீட்டிற்குள் — recovering the meaning that
[token explosion](../01-origin/03-how-llms-read-words.md) destroys.

## Morphological lift — our north-star metric

Claims are cheap; we measure. The product metric is the **A/B delta**:

```
lift = score(model WITH thamizh-mcp) − score(same model WITHOUT)
```

- **Question set:** ILAKKANAM-style Tamil linguistics questions (school-exam form, linguist-verified
  answers) spanning the benchmark's categories — phonetics, phonology, **morphology**, syntax,
  semantics, fact — and grade bands. (The original ILAKKANAM dataset isn't public yet; we build and
  hand-verify fixtures in its exact format, and will adopt the original as a held-out test the day
  it's released.)
- **Protocol:** same model, same prompts, zero-shot, multiple runs per question; the control arm runs
  where the server is genuinely unreachable; misses are manually reviewed for linguistically
  acceptable variants before scoring.
- **Reported honestly:** per category and grade band, never one blended number. Where the tools don't
  help (expected for e.g. syntax — a word-level server), we say so; a *negative* lift is a finding
  that routes work, not a number to hide. Benchmark words are flagged in the store so they can
  [never leak into published training data](03-data-curation.md).

**Why we expect real lift where it counts:** the benchmark's own findings show models are weakest
exactly where our anchors are strongest — morphology and upper-grade grammar. And open-source models
(scoring 38–61%) have the most room to gain, which matters for a public-good project: grounding tools
narrow the gap between free models and frontier ones.

→ Next: [Data curation — the flywheel](03-data-curation.md) · [Roadmap](05-roadmap.md)
