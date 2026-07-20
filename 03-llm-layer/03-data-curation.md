# Data curation — the flywheel toward a Tamil language model

> Every grounded answer the server gives is also a verified data point. Curated carefully, those
> points become the gold-standard datasets Tamil AI has never had.

## Why data is the endgame

High-resource languages are high-resource because of **data** — and the scarcest Tamil AI data isn't
raw text (the web has plenty) but **verified linguistic structure**: which morphemes make up a word,
what a word's origin is, which native equivalent is actually attested. Our survey of public dataset
hubs found **zero** Tamil datasets of these kinds. They are precisely what a grammar-aware tokenizer
and a future Tamil language model need — and precisely what this server produces as a by-product.

## The pipeline

```
transactions log            every completed analysis, recorded with provenance
  → verify                  three bins by evidence strength:
       gold      anchor-grounded (FST / pinned lexicon / rule table)
       silver    evolving-only (e.g. Wiktionary-sourced, cross-check pending) — labelled as such
       disputed  competing scholarly claims — kept as a split, never merged into gold
  → round-trip check        a segmentation is gold ONLY if the FST regenerates the surface form
                            from the claimed parts (மரம்+அத்து+இல் → மரத்தில் ✓)
  → dedupe + normalize      NFC, one row per (word, claim); earliest provenance kept
  → contamination guard     benchmark-fixture words (flagged in the store) are dropped —
                            published training data must never contain our eval questions
  → license gate            only records whose sources permit redistribution; we publish derived
                            structured facts with citations, never bulk third-party text
  → export                  versioned JSONL datasets with a provenance column on every row
```

## The four dataset shapes

1. **Morphological segmentation** — surface form → parts with roles (பகுதி/சாரியை/உருபு…) + sandhi
   notes. *The training input for a grammar-first Tamil tokenizer.*
2. **Origin labels** — word → இயற்சொல்/வடசொல்/loan + source language, disputes preserved in their own
   split (a research feature, not noise).
3. **Loanword → native equivalents** — ranked attested equivalents with register and attestation
   source.
4. **Instruction records** — Q/A pairs about words, generated **only by filling fixed templates with
   verified fields**. An LLM may rephrase a *question*; it never produces an *answer* fact. The
   no-LLM-as-source principle, extended to data.

## Where the data accumulates

A locally installed server accumulates only its own usage. The **hosted public instance**
([deployment](../02-engineering/06-deployment.md)) sees all its users' queries, making it the central
accumulator; the durable artifact is the **versioned public dataset** on Hugging Face — git carries
code and pinned anchors, HF carries the grown corpus. (A community "contribute your local data"
feature is deliberately deferred until its consent, privacy, and licensing model is designed.)

Quality bars, in one line: every record traceable to a named source; zero LLM-originated facts;
uneven coverage stated honestly on the dataset card rather than padded away.

→ Next: [The Hugging Face plan](04-huggingface-plan.md) · [Roadmap](05-roadmap.md)
