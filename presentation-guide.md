# Presentation guide — walking this wiki as a talk

Three ready-made tracks. Each bullet is one wiki article ≈ one talk segment; present the article's
tables/diagrams directly.

## Track A — Tamil native speakers, no IT background (≈25 min, தமிழில்)

1. **Hook (2 min):** live or screenshot demo — ask an AI a பகுபத உறுப்பு question, show it stumble.
2. [ஏன் இந்தத் திட்டம்](01-origin/01-why-this-project.md) (5 min) — the ILAKKANAM score table is the
   slide: *"பள்ளித் தமிழ்த் தேர்வில் AI தோற்கிறது."*
3. [தமிழ் இலக்கணம் — AI கண்ணோட்டத்தில்](01-origin/02-tamil-grammar-primer.md) (7 min) — the audience's
   home turf; walk மரத்தில் end-to-end. Let *them* correct the AI — it lands the point.
4. [AI எப்படி சொற்களைப் படிக்கிறது](01-origin/03-how-llms-read-words.md) (5 min) — bead-chain analogy;
   "in the house" vs வீட்டிற்குள் side by side.
5. [எங்கள் அணுகுமுறை](01-origin/04-our-approach.md) (5 min) — the ask-don't-guess flow; close on
   *நேர்மையான இடைவெளிகள்* (AI that says "தெரியவில்லை") — reliably the most appreciated idea.
6. **Close (1 min):** the [flywheel](03-llm-layer/03-data-curation.md) in one sentence — "நீங்கள்
   பயன்படுத்தும் ஒவ்வொரு கேள்வியும் எதிர்காலத் தமிழ் AI-க்கான தங்கத் தரவாகிறது."

## Track B — Tamil AI enthusiasts / developers (≈45 min, English + Tamil examples)

1. Problem: [Why this project](01-origin/01-why-this-project.md) + the token half of
   [How LLMs read words](01-origin/03-how-llms-read-words.md) (8 min).
2. Grammar-as-spec: skim [the primer](01-origin/02-tamil-grammar-primer.md) — frame it as *the output
   contract*, not a lesson (5 min).
3. [What is a GPT](03-llm-layer/01-what-is-a-gpt.md) → [MCP meets LLM](03-llm-layer/02-mcp-meets-llm.md)
   (8 min) — the ask-don't-guess flow + **morphological lift** as the falsifiable claim.
4. Engineering core: [Architecture](02-engineering/01-architecture.md) →
   [FST](02-engineering/02-fst-thamizhimorph.md) → [MCP tools](02-engineering/04-mcp-tools.md)
   (12 min) — live `analyze_word` demo here if possible.
5. [Data curation](03-llm-layer/03-data-curation.md) + [HF plan](03-llm-layer/04-huggingface-plan.md)
   (7 min) — the white-space finding ("no Tamil segmentation gold exists anywhere") wakes rooms up.
6. [Roadmap](03-llm-layer/05-roadmap.md) + contribution asks (5 min).

## Track C — deep technical session (60+ min)

Track B, plus: [Python stack & testing](02-engineering/03-python-stack.md) ·
[Sources & provenance](02-engineering/05-sources-provenance.md) ·
[Deployment ladder](02-engineering/06-deployment.md). Live-code: run the server under MCP Inspector,
show a cache-miss → evolving pull → write-back cycle, and a deliberate honest-gap response.

## Speaker notes

- **Always open with மரத்தில், not theory** — one word carrying sandhi, case ambiguity, and a clean
  six-part split sells the whole project.
- **Show a failure honestly** (e.g. origin `unknown` for a pure-script loan) and then the roadmap item
  fixing it — credibility beats polish.
- Keep the [glossary](glossary.md) open for mixed audiences.
- Expected hard questions: *"Why not just fine-tune?"* →
  [What is a GPT §two ways](03-llm-layer/01-what-is-a-gpt.md) + [Roadmap §why last](03-llm-layer/05-roadmap.md).
  *"Who decides what's தூய தமிழ்?"* → we don't; we report attested sources and registers, disputes
  included ([primer §2.1](01-origin/02-tamil-grammar-primer.md)).
