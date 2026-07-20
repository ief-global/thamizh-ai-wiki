# What is a GPT? — a plain explanation

> No math, no code. Enough to understand why fluent Tamil output doesn't mean Tamil knowledge —
> and why a grounding tool changes the game. தமிழில் அடிப்படை:
> [AI எப்படி சொற்களைப் படிக்கிறது](../01-origin/03-how-llms-read-words.md).

## The core trick: predict the next piece

A GPT (Generative Pre-trained Transformer) — the technology behind ChatGPT, Claude, Gemini — is, at
heart, a machine trained on one task: **given some text, predict what comes next.** Feed it "வணக்கம்,
எப்படி…" and it predicts "…இருக்கிறீர்கள்" because, across the oceans of text it was trained on, that
continuation was most likely.

Do this prediction over and over, one [token](../01-origin/03-how-llms-read-words.md) at a time, and
you get paragraphs, poems, code, answers. Scale the model and the training text up enormously, and the
predictions become so good they *look like* understanding — often usefully so.

## Why fluency ≠ knowledge

The model holds **statistical patterns**, not a library of checked facts. Three consequences matter
for Tamil:

1. **It answers from exposure.** If Tamil grammar discussions were thin in its training data — and for
   deep இலக்கணம் they are — the pattern is weak, and the model improvises. The
   [ILAKKANAM benchmark](../01-origin/01-why-this-project.md) caught exactly this: models score like
   students who memorized past papers but never learned the subject; accuracy collapses as grade level
   rises, and their overall score doesn't even correlate with recognizing what *kind* of linguistic
   question they're facing.
2. **It fills gaps confidently ("hallucination").** Prediction machines abhor a vacuum: ask for a pure
   Tamil equivalent that doesn't exist, and a GPT will happily *coin* a plausible-sounding word.
   Fluent, authoritative, wrong — the most dangerous failure mode for language questions.
3. **English gets the best of it.** English dominates training corpora AND the tokenizer, so English
   answers ride strong patterns over clean tokens. Tamil rides weak patterns over
   [shattered tokens](../01-origin/03-how-llms-read-words.md). The playing field is not level.

## The two ways to fix a knowledge gap

1. **Retrain / fine-tune the model** on more Tamil — powerful but slow and expensive (this is the
   [long-term SLM goal](05-roadmap.md)), and it still doesn't give *verifiable* answers.
2. **Give the model tools** — let it *look things up* at answer time and cite what it found. This is
   cheap, immediate, works with every current model, and produces auditable answers.

Thamizh MCP is path 2, built so its by-products make path 1 possible later —
[the data flywheel](03-data-curation.md).

→ Next: [When MCP meets the LLM](02-mcp-meets-llm.md)
