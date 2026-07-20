# FST & ThamizhiMorph — the machine that knows புணர்ச்சி

> The single most important component we build on: a finite-state transducer that analyses and
> generates Tamil word forms by rule. Grammar background:
> [Tamil grammar primer](../01-origin/02-tamil-grammar-primer.md).

## What is a finite-state transducer?

A **finite-state machine** is the simplest possible computer: a set of states with labelled arrows;
input walks you arrow by arrow. A **transducer** (FST) is that machine with **two tapes** — it reads
one string and writes another, mapping between two levels of representation:

```
surface form  (what you see):        மரத்தில்
                                        ⇅   (one FST, both directions)
lexical form  (root + features):     மரம் +Noun +Sg +Loc
```

Two properties make FSTs the right tool for Tamil morphology:

1. **They are bidirectional.** The *same* rule set runs both ways:
   - **Analysis** (surface → lemma + POS + features): மரத்தில் → மரம், noun, locative — what our
     server uses on every query.
   - **Generation** (lemma + features → surface): மரம் + locative → மரத்தில் — which lets rules
     produce **millions of valid word forms from a few thousand roots + paradigms**, with zero
     per-word maintenance. Generation is also our data-quality check: a segmentation is trusted only
     if the FST can regenerate the surface form from the claimed parts
     ([round-trip validation](../03-llm-layer/03-data-curation.md)).
2. **They encode sandhi natively.** புணர்ச்சி changes (மரம்→மரத், வா→வந்) are exactly the kind of
   context-dependent string rewriting FSTs were invented for. The rules compile *into* the machine, so
   analysis undoes sandhi and generation applies it — deterministically, by rule, not by statistics.

## ThamizhiMorph — the FST we wrap

**ThamizhiMorph** (Sarveswaran, Dias & Butt, *Machine Translation* 2021; Apache-2.0) is an open-source
Tamil morphological analyser-generator built as a foma FST:

- A high-level **meta-language** compiles Tamil inflectional morphology from linguist-defined
  **paradigms** (verbal classes; nominal classes incl. 38 pronoun classes), following Lehmann (1993)
  and the traditional grammars.
- Evaluated on a school-textbook corpus: **93.3% analysis coverage**, and among successful analyses
  **100% correct analysis / 97.9% correct lemma** — residual failures are almost all out-of-vocabulary
  roots, which are *addable* (bounded stem additions, not the banned hand-maintained word list).
- Queried through **foma's `flookup`** binary — a native C tool, one subprocess call per lookup.

**Why wrap it rather than build our own?** Rebuilding would duplicate mature, openly licensed research
for no gain. Our value is the layer ThamizhiMorph never attempted: orchestration, provenance,
Tholkappiyam decoding, equivalents, honest gaps, and the MCP interface. (MCP didn't exist until 2024;
ThamizhiMorph is 2018–2021 research.)

**Policies we add on top:**
- **Guesser FSTs excluded.** ThamizhiMorph ships "guesser" transducers that hypothesize analyses for
  unknown words. We turn them off: an unknown word routes to the enrichment loop or an honest gap —
  never to a guess dressed as an analysis.
- **All analyses kept.** The FST returns every valid parse (மரத்தில் → loc *and* soc readings);
  we surface all of them with provenance.
- **Tags decoded into the tradition.** Raw FST tags (`+Loc`, `+Past+3SM`) are decoded once, in tested
  code, into பகுபத உறுப்புகள், வேற்றுமை ordinals, and வினைமுற்று descriptions — with the
  Tholkappiyam/Nannūl authority recorded per claim. The LLM never re-derives grammar.

→ Next: [Python stack & testing](03-python-stack.md) · [The MCP tools](04-mcp-tools.md) ·
[Sources & provenance](05-sources-provenance.md)
