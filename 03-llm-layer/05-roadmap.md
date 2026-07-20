# Roadmap & milestones

> Sequenced so every near-term deliverable is immediately useful AND a prerequisite asset for the
> next phase. The Tamil language model comes last **by design**, not neglect.

## Done so far (mid-2026)

| Milestone | Status |
|---|---|
| Design blueprint: five objectives, source tiers, output contract, honesty rules | ✅ signed off |
| ThamizhiMorph FST anchor integrated (pinned), foma pipeline | ✅ |
| Knowledge store with per-claim provenance + enrichment loop | ✅ |
| **All 9 v1 MCP tools live** ([tool reference](../02-engineering/04-mcp-tools.md)) | ✅ |
| 87-test suite; honest-gap and ambiguity behaviours tested | ✅ |
| Transaction logging (the data flywheel) with eval-contamination guard | ✅ |
| Tamil Wiktionary adapter parsing real entry structure | ✅ |

## Near term (active)

1. **Origin-classifier accuracy lift** — wire the Thamizhi Word Validator + a vendored loanword
   dataset, so pure-script borrowings (புத்தகம்-class words) resolve instead of returning honest
   `unknown`.
2. **Run the morphological-lift benchmark** — build the ILAKKANAM-style fixture set, run the
   [A/B protocol](02-mcp-meets-llm.md), publish per-category results. The flagship proof.
3. **Source snapshots** — pinned offline copies of the Madras Tamil Lexicon + கலைச்சொல் glossaries
   (terms permitting); locate and license-verify the **Aalamaram** treebank.
4. **Verse-level classical citations** — pin digitized Tholkappiyam + Nannūl editions as anchor data
   and upgrade grammar citations from section names to exact **நூற்பா numbers**
   ([how grounding works](../02-engineering/05-sources-provenance.md)).
5. **First public release** — license audit, then PyPI + Docker
   ([release ladder](../02-engineering/06-deployment.md)), then registry + Tamil-NLP catalog listings.
6. **Hugging Face org + dataset v0** — lock the namespace and card discipline early
   ([HF plan](04-huggingface-plan.md)).

## Medium term

6. **Hosted public instance + web tool** — the rung that reaches non-technical Tamil speakers; run as
   a non-profit public service under the `ief-global` umbrella. Central data accumulation begins.
7. **Full புணரியல் sandhi engine** — name every தோன்றல்/திரிதல்/கெடுதல் (including verb-root changes
   like வா→வந்), upgrading the deliberately conservative v1 formation output.
8. **Aalamaram integration** — treebank-scale cross-checking of analyses; richer eval fixtures;
   groundwork for phrase-level support.
9. **Phrase/sentence analysis (v2)** — contextual POS disambiguation resolves ambiguities
   (மரத்தில்: locative or ablative?) that single-word analysis correctly refuses to.
10. **RAG on de-agglutinated roots** — semantic search that indexes morpheme roots instead of
    shattered tokens; growing HF dataset versions.

## Long term — the Tamil SLM (deliberately last)

A **small language model for Tamil**: grammar-first tokenizer trained on our segmentation gold (so
Tamil words tokenize along real morpheme boundaries), continued pretraining of a compact open model on
quality Tamil corpora, instruction-tuning on our verified records.

**Why last:** training a credible SLM needs three things that don't exist until the earlier phases
create them — a large verified corpus (the flywheel), a trusted evaluation
(the lift benchmark + fixtures), and a community of users (the released server). Attempting it first
would mean training on unverified data and evaluating on nothing. The near-term work isn't a detour
from the SLM; it **is** the SLM's missing foundation.

Also long-term: Tanglish/code-mix support, mobile apps, offline on-device analysis.

## Follow along / contribute

Code and issues: [`thamizh-mcp`](https://github.com/ief-global/thamizh-mcp). The most valuable early
contributions: OOV word reports, disputed-origin evidence, attested-equivalent sources, and eval
fixtures from real school papers.
