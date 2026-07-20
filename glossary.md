# Glossary / கலைச்சொல் அகராதி

Two tables: Tamil grammar terms as the server uses them, and AI/engineering terms with a Tamil gloss.
Deep dives: [Grammar primer](01-origin/02-tamil-grammar-primer.md) ·
[How LLMs read words](01-origin/03-how-llms-read-words.md).

## தமிழ் இலக்கணச் சொற்கள் / Tamil grammar terms

| தமிழ் | English | குறிப்பு |
|---|---|---|
| இலக்கணம் | grammar | |
| தொல்காப்பியம் | Tholkappiyam | முதன்மை ஆதார நூல் — our golden authority |
| நன்னூல் | Nannūl | fallback authority (six-part பகுபதம் labels) |
| பெயர்ச்சொல் | noun | |
| வினைச்சொல் | verb | |
| இடைச்சொல் | particle / clitic | case suffixes, postpositions, emphatics |
| உரிச்சொல் | qualifier | adjective/adverb-like |
| இயற்சொல் | native word (common use) | |
| திரிசொல் | native word (literary/shifted) | |
| திசைச்சொல் | regional/Dravidian word | |
| வடசொல் | Sanskrit/Indo-Aryan loanword | தற்சமம் (unchanged) / தற்பவம் (adapted) |
| கடன்சொல் | loanword (modern: English, Portuguese…) | outside the classical four-way scheme |
| தூய தமிழ் இணை / கலைச்சொல் | native equivalent / technical term | attested-only in our system |
| வேர்ச்சொல் | root word | |
| பகாப்பதம் | unanalyzable word | மண், கல் |
| பகுபதம் | analyzable word | splits into உறுப்புகள் |
| பகுதி · விகுதி · இடைநிலை · சாரியை · சந்தி · விகாரம் | root · suffix · medial(tense) · augment · juncture · join-change | the six parts (Nannūl) |
| வேற்றுமை | (noun) case | eight cases; உருபு = case suffix |
| வினைமுற்று | finite verb | tense + person-number-gender |
| புணர்ச்சி | sandhi (joining rules) | தோன்றல்/திரிதல்/கெடுதல் |
| எழுத்ததிகாரம் / சொல்லதிகாரம் | Tholkappiyam's books on letters / words | where each rule lives |

## AI & engineering terms / AI-கலைச்சொற்கள்

| Term | English meaning | தமிழ்க் குறிப்பு |
|---|---|---|
| LLM / GPT | large language model — next-token prediction at scale | அடுத்த சொல்-துண்டை ஊகிக்கும் பெருமொழி மாதிரி → [explainer](03-llm-layer/01-what-is-a-gpt.md) |
| Token | the text fragment unit an LLM reads | AI படிக்கும் சொல்-துண்டு / "மணி" |
| Tokenizer | the splitter that cuts text into tokens | சொற்களை துண்டாக்கும் "கத்தி" |
| Token explosion | Tamil words shattering into many meaningless tokens | தமிழ்ச்சொல் சிதறல் சிக்கல் |
| Agglutinative | language that glues morphemes into one word | ஒட்டுநிலை மொழி (தமிழ்) |
| Analytic | language using separate words for grammar | தனிச்சொல் மொழி (ஆங்கிலம்) |
| Hallucination | confident fabrication by an LLM | AI-யின் நம்பிக்கையான கற்பனை பதில் |
| Grounding | answering from verifiable sources | ஆதாரத்துடன் பதிலளித்தல் |
| Provenance | the recorded source of each claim | ஒவ்வொரு கூற்றின் ஆதாரத் தடம் |
| MCP | Model Context Protocol — standard for LLM↔tool connection | AI-கருவி இணைப்பு நெறிமுறை |
| MCP server / tool | a program exposing callable tools to an LLM | AI அழைக்கும் கருவித் தொகுப்பு |
| FST | finite-state transducer — two-tape rule machine | இரு-திசை விதி இயந்திரம் → [explainer](02-engineering/02-fst-thamizhimorph.md) |
| Lemma | dictionary/base form of a word | அகராதி அடிவடிவம் (≈ வேர்ச்சொல்) |
| POS | part of speech | சொல் வகை |
| Morphology | study of word formation | சொல்லுருவாக்கவியல் (சொல் இலக்கணம்) |
| ThamizhiMorph | the open-source Tamil FST we build on | |
| foma / flookup | FST compiler / lookup tool (C binary) | |
| FastMCP · uv · SQLite · Pydantic | Python MCP framework · package manager · embedded database · schema validation | → [stack](02-engineering/03-python-stack.md) |
| RAG | retrieval-augmented generation (search + LLM) | தேடல்-இணைந்த பதிலாக்கம் |
| Fine-tuning / SLM | further training a model / small language model | மாதிரி மேம்பயிற்சி / சிறு மொழி மாதிரி |
| Morphological lift | our metric: score(with tools) − score(without) | கருவியால் கிடைக்கும் மேம்பாட்டு அளவு |
| Hugging Face | the ML community's model/dataset hub | → [plan](03-llm-layer/04-huggingface-plan.md) |
| Benchmark / ILAKKANAM | standardized test set / the Tamil linguistics benchmark | தரப்படுத்தப்பட்ட தேர்வு |
