# The MCP tools — what the server actually exposes

> Nine tools, one contract. Every tool returns provenance-tagged fields from the same engine and
> reports honest gaps. Background: [Architecture](01-architecture.md).

## The flagship: `analyze_word`

One call, the complete picture — composes all the focused tools and returns the full **WordAnalysis**
object: normalization, origin, root+meaning, formation, grammar, native equivalent (when applicable),
per-section gaps, and the provenance table. This is what an AI assistant usually wants: *everything
about this word, sourced.*

```
analyze_word("மரத்தில்") →
  lemma: மரம் (ThamizhiMorph, anchor)
  word class: பெயர்ச்சொல் (Tholkappiyam சொல்லதிகாரம்)
  formation: மரம் [பகுதி] + அத்து [சாரியை] + இல் [உருபு] — labels per Nannūl
  case: ஏழாம் வேற்றுமை (locative) AND ஐந்தாம் (ablative) — both kept, ambiguity stated
  origin: இயற்சொல்
  meaning: (store/Wiktionary, with retrieval date) — or an honest gap note
```

## The focused tools

| Tool | Question it answers | Notes |
|---|---|---|
| `classify_origin` | இயற்சொல் / திரிசொல் / திசைச்சொல் / வடசொல் / modern loan? | rule-based: Grantha-letter detection, Tholkappiyam phonotactics (which letters can begin/end a native word), FST parseability, attested loan lists. Returns `unknown` honestly when markers are absent — see [roadmap](../03-llm-layer/05-roadmap.md) for the accuracy lift under way |
| `get_root` | lemma + part of speech | all FST analyses kept, never silently one |
| `get_meaning` | dictionary sense(s) | store first, then evolving pull; senses carry source + date |
| `explain_formation` | பகுபத உறுப்பு split + sandhi events | joins are *named* only where a confident classical rule applies — no invented splits |
| `explain_grammar` | word class, வேற்றுமை, tense/PNG for verbs | every claim carries its authority (Tholkappiyam / Nannūl) |
| `suggest_native_equivalent` | attested தூய தமிழ் equivalent | runs only for non-native words; candidates ranked with source + register; empty + note when nothing is attested |
| `enrich_word` | force a fresh pull for one word | writes back to the store (marked non-read-only) |
| `refresh_sources` | batch re-pull (stale entries / word list) | maintenance; non-read-only |

**Where's the "normalizer"?** Normalization (Unicode NFC, Tamil-script validation) is a pipeline
*stage* every tool runs first, not a separate tool — malformed or non-Tamil input is rejected with a
clear error before any analysis.

**Planned (post-v1):** `validate_pure_tamil` (is this text free of loanwords?), `generate_forms`
(the FST's generation direction as a tool), `transliterate`.

## Design details that matter to integrators

- **Read-only hints:** analysis tools are marked read-only; `enrich_word`/`refresh_sources` are the
  only writers — clients can gate them accordingly.
- **Errors teach:** a failed grounding names the missing source ("no lexicon entry; evolving pull
  timed out") so the calling LLM can tell the user *why* — and never fills the void itself.
- **Every response is schema-valid** against the published WordAnalysis JSON Schema — stable field
  names, machine-checkable.

→ Next: [Sources & provenance](05-sources-provenance.md) ·
[How an LLM uses these tools](../03-llm-layer/02-mcp-meets-llm.md)
