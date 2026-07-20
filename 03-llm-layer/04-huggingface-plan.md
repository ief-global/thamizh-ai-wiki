# The Hugging Face plan — ief-global on the ML commons

## What Hugging Face is (and why it matters here)

**Hugging Face (HF)** is the GitHub of machine learning: the place the ML community hosts and
discovers **models**, **datasets**, and **Spaces** (small hosted demo apps). Data or models published
there are versioned, carry documentation cards, and plug directly into standard training tools. For a
low-resource-language project, HF is where your work becomes *usable by everyone else's training
runs* — publication there is contribution to the commons, not just storage.

## Division of labor: GitHub × Hugging Face

| Platform (`ief-global` on both) | Carries |
|---|---|
| **GitHub** | the server's code (Apache-2.0), pinned anchor data, releases, this wiki |
| **Hugging Face** | the **curated datasets** (gold/silver/disputed splits, versioned, carded) · a **Space** demo · long-term, the Tamil SLM |

## The white space we fill

Surveying HF's Tamil shelf (2026): strong in speech (ASR/TTS corpora), raw web text, sentiment, and
Llama/Mistral Tamil fine-tunes — but **no morphological segmentation gold, no loanword→equivalent
dataset, no origin-label dataset.** Our [four dataset shapes](03-data-curation.md) are first-of-kind
there. That's both a contribution and an invitation: researchers who need Tamil morphology data will
find it under `ief-global`, with every row citing its source.

## What we publish, and how

- **Datasets first, small and early:** version 0 ships with hundreds of verified records rather than
  waiting for millions — locking in the card discipline (per-subset licenses, provenance column,
  method notes, known-coverage gaps, contamination statement) from day one. Versions grow as the
  [flywheel](03-data-curation.md) turns.
- **A Space as the public demo:** a simple page — type a Tamil word, see the sourced analysis — that
  calls the hosted instance's API (one backend; the Space is a thin front-end). This puts a live demo
  where the ML community already browses.
- **Existing HF assets flow back in:** Tamil corpora on HF (textbook collections, cleaned web text,
  Wikipedia dumps) become pretraining material in the SLM era, and HF-hosted Indic benchmarks join
  our evaluation context alongside [morphological lift](02-mcp-meets-llm.md).
- **Not doing:** mirroring other people's models under the org, or publishing anything whose license
  audit hasn't cleared.

## The long game

When the corpus and evaluation infrastructure mature, `ief-global` on HF is where the **Tamil small
language model** lives — trained with a grammar-first tokenizer built on our segmentation gold,
instruction-tuned on our template-generated records, evaluated on the benchmarks this project
normalized. See [Roadmap](05-roadmap.md) for why that's deliberately sequenced last.

→ Next: [Roadmap & milestones](05-roadmap.md)
