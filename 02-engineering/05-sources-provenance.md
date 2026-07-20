# Sources & provenance — what grounds every answer

> The trust model: two tiers of sources, a provenance record on every claim, and cross-check
> discipline that keeps growth from becoming drift.

## Two tiers

**Anchors** — stable authorities, **version-pinned** (a claim is reproducible only if its source
version is known):

| Anchor | Grounds | License / access |
|---|---|---|
| தொல்காப்பியம் rule table (encoded in code) | word classes, origin classes, வேற்றுமை, புணர்ச்சி | classical text; our encoding Apache-2.0; verse-level citations planned (below) |
| நன்னூல் (labels only) | the six-part பகுபதம் scheme | classical text |
| ThamizhiMorph FSTs (pinned commit) | lemma, POS, formation, sandhi | Apache-2.0, cited |
| Madras Tamil Lexicon (planned pinned snapshot) | meanings | terms under review before shipping |
| கலைச்சொல் glossaries (planned pinned snapshots) | native equivalents | per-glossary review |
| Aalamaram treebank (adopted, pending) | morphology cross-checks, eval material | license being verified |

**Evolving** — community-fed, growing; pinned by **retrieval date + provenance** instead of version:

| Source | Grounds | Note |
|---|---|---|
| Tamil Wiktionary | meanings, synonyms (சொல்வளம் templates), etymology hints | CC BY-SA — cached locally, never redistributed raw |
| Indic-To-Pure-Tamil lists (pinned commit) | loanword → equivalent mappings | small & older, but attested; supplemented, not replaced |
| Loanword datasets | origin evidence | licenses reviewed per set |

## The provenance record

Every filled field stores: **source name · tier · version-or-retrieval-date**. So an assistant can
answer: *"root per ThamizhiMorph (anchor); meaning per Tamil Wiktionary, retrieved 2026-07-03; native
equivalent கணினி per the terminology glossary."* An answer that can't cite is an answer we don't give.

## How classical texts ground a computer program

Neither the LLM nor the running server "reads" தொல்காப்பியம். The chain is: the classical text defines
the categories → humans encoded those rules **once, at design time**, into a fixed rule table in tested
code, each entry citing its authority and section → at runtime the deterministic table fires and the
claim ships with its citation → the LLM merely relays it. Grammar is never re-derived by a model.

**Planned hardening — verse-level (நூற்பா) citations:** today the rule table cites at *section* level
("தொல்காப்பியம், வேற்றுமையியல்"). The design now calls for pinning digitized **Tholkappiyam and Nannūl
editions** as version-locked anchor artifacts (candidate sources: Project Madurai, Tamil Virtual
Academy — the gold edition is chosen and recorded at pinning time) and upgrading every rule's citation
to the exact **நூற்பா number**. Then a scholar can audit any grammar claim to the verse, the same way a
developer can audit an FST claim to a pinned commit. Until that lands, citations are honestly
section-level.

## Cross-check discipline

An evolving-tier fact is promoted to a grounded claim only if it's attributable AND consistent with an
anchor or classical rule; otherwise it surfaces explicitly as low-confidence. Disputed origins
(உலகம் — native உலகு vs Sanskrit *loka*) return **both positions with their evidence**. This is how the
store grows with use without drifting into unsourced folklore.

## Licensing stance (public-release gate)

Our code is Apache-2.0, but we stand on data with varied terms — so a **license audit blocks every
public release rung**: attribution for Apache-2.0 components (ThamizhiMorph cited in NOTICE +
CITATION), share-alike care for Wiktionary-derived content (cache is local; anything *published* is
derived structured facts with attribution, never bulk text), and written-terms checks before any
lexicon/glossary snapshot ships. Datasets we publish declare per-subset licenses on their cards —
see [Data curation](../03-llm-layer/03-data-curation.md).

→ Next: [Deployment](06-deployment.md) · [The Hugging Face plan](../03-llm-layer/04-huggingface-plan.md)
