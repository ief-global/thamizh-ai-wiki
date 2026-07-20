# Thamizh AI — Knowledge Base / அறிவுக் களஞ்சியம்

**தமிழ் மொழியை AI உலகில் ஆங்கிலத்திற்கு இணையான இடத்திற்கு கொண்டு செல்லும் திட்டம்.**
An open project to make Tamil a first-class language in the AI/LLM ecosystem — starting with
**Thamizh MCP**, a tool server that grounds AI answers about Tamil words in the authentic grammatical
tradition (தொல்காப்பியம் முதல்).

> Sister repo: [`thamizh-mcp`](https://github.com/ief-global/thamizh-mcp) — the open-source server this
> wiki explains. Run as a public good by a **non-profit initiative under the `ief-global` umbrella**.

## Why this exists, in one paragraph

Today's AI models are fluent in Tamil but not *grounded* in it: on a benchmark of school-exam Tamil
grammar questions, even the best models score ~71–80% and fall further as questions get harder. Two root
causes: (1) AI tokenizers shred agglutinated Tamil words into meaningless fragments, and (2) models
answer from statistical exposure, not from Tamil's grammatical authorities. Thamizh MCP fixes this
architecturally — the AI *asks a tool* that answers from தொல்காப்பியம், a rule-based morphological
analyser, and attested lexicons, with every claim carrying its source. No model retraining required —
and every answered query quietly builds the gold-standard dataset a future Tamil language model needs.

## The three sections

| Section | For whom | Language |
|---|---|---|
| **[1. Origin / தோற்றம்](01-origin/01-why-this-project.md)** — why Tamil needs this, Tamil grammar ↔ English grammar, how AI reads words, our remedy | Native Tamil speakers — **no IT background needed** | தமிழ் முதன்மை (Tamil-first, English recaps) |
| **[2. Engineering](02-engineering/01-architecture.md)** — FST, Python stack, the MCP tools, sources, deployment | Developers | English |
| **[3. The LLM layer](03-llm-layer/01-what-is-a-gpt.md)** — GPTs explained, how the server lifts AI quality, data curation, Hugging Face | AI enthusiasts | English |

## Article map

**01 · Origin / தோற்றம்**
[Why this project / ஏன் இந்தத் திட்டம்](01-origin/01-why-this-project.md) ·
[Tamil grammar primer / தமிழ் இலக்கணம்](01-origin/02-tamil-grammar-primer.md) ·
[How AI reads words / AI எப்படி சொற்களை படிக்கிறது](01-origin/03-how-llms-read-words.md) ·
[Our approach / எங்கள் அணுகுமுறை](01-origin/04-our-approach.md)

**02 · Engineering**
[Architecture](02-engineering/01-architecture.md) ·
[FST & ThamizhiMorph](02-engineering/02-fst-thamizhimorph.md) ·
[Python stack & testing](02-engineering/03-python-stack.md) ·
[The MCP tools](02-engineering/04-mcp-tools.md) ·
[Sources & provenance](02-engineering/05-sources-provenance.md) ·
[Packaging & deployment](02-engineering/06-deployment.md)

**03 · LLM layer**
[What is a GPT?](03-llm-layer/01-what-is-a-gpt.md) ·
[When MCP meets the LLM](03-llm-layer/02-mcp-meets-llm.md) ·
[Data curation — the flywheel](03-llm-layer/03-data-curation.md) ·
[The Hugging Face plan](03-llm-layer/04-huggingface-plan.md) ·
[Roadmap & milestones](03-llm-layer/05-roadmap.md)

**Reference:** [Glossary / கலைச்சொல் அகராதி](glossary.md) · [Presentation guide](presentation-guide.md)

## Status snapshot (2026-07)

v1 core **complete**: 9 MCP tools live, 87 tests passing, provenance-tagged knowledge store with
transaction logging. Next: origin-classifier lift, the morphological-lift benchmark, first public
release. Details: [Roadmap & milestones](03-llm-layer/05-roadmap.md).

## Credits

Standing on the shoulders of open Tamil-NLP research — chiefly **ThamizhiMorph** (Sarveswaran, Dias &
Butt 2021), the **ILAKKANAM** benchmark (Univ. of Jaffna, 2025), **open-tamil**, **Indic-To-Pure-Tamil**,
and the **Aalamaram** treebank (WILDRE 2024). Full catalog with licenses:
[Sources & provenance](02-engineering/05-sources-provenance.md). Code: Apache-2.0.
