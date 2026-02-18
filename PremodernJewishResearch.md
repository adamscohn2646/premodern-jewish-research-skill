---
name: premodern-jewish-research
description: Research premodern Jewish texts using Sefaria. Triggers when user asks to find, analyze, or research Jewish texts, commentaries, or topics from before 1800. Handles complete workflow from searching Sefaria to fetching and analyzing texts in original languages with historical context. Use for classical Jewish sources, biblical commentary, Talmud, Midrash, medieval philosophy, and early modern texts.
---

# Premodern Jewish Research (Sefaria MCP)

Research premodern Jewish texts (pre-Haskalah, roughly pre-1800) using Sefaria's MCP tools. This skill provides direct access to Sefaria's text database, search engines, dictionaries, and cross-reference system. No URL-pasting workflow required.

## Available Tools

Claude has direct MCP access to Sefaria with these tools:

| Tool | Purpose | When to Use |
|------|---------|-------------|
| get_text | Retrieve text by reference | When you know the exact citation |
| get_links_between_texts | Find cross-references and commentaries | To discover what is linked to a verse |
| text_search | Keyword search across the library | For Hebrew/Aramaic term searches |
| search_in_book | Search within one specific work | For targeted searches in Zohar, Talmud, etc. |
| english_semantic_search | Conceptual/thematic search in English | For discovering sources by idea rather than keyword |
| search_in_dictionaries | Look up terms in BDB, Klein, Jastrow | For etymology and root analysis |
| get_topic_details | Get information about a topic | For biographical and thematic context |
| get_text_catalogue_info | Get metadata about a text | To understand a work's structure |
| get_text_or_category_shape | Get structural overview | To see what sections exist |
| clarify_name_argument | Validate/autocomplete text names | When unsure of exact Sefaria title |
| get_english_translations | Get all English translations | When comparing translation choices |
| get_available_manuscripts | Find manuscript images | For textual criticism or visual evidence |
| get_current_calendar | Jewish calendar info | For parasha, holiday context |

## Search Strategy: What Works

### Rule 1: Hebrew First, Always

Hebrew and Aramaic searches are dramatically more reliable than English. English searches are hit-or-miss due to translation variation across editions.

Good: text_search with Hebrew like שבעת הסריסים (seven eunuchs)
Bad: text_search with English like "seven eunuchs of Ahasuerus"

Good: search_in_dictionaries with המה (root form)
Bad: search_in_dictionaries with "tumult" or "Haman"

When you must search in English, use english_semantic_search. It is designed for conceptual matching and handles translation variation better than keyword search.

**CRITICAL: Never mix Hebrew and English in the same text_search query.** Pick one language per query. Mixed-language queries like "decree sealed כתיבה גזירה sefirotic" return noise or nothing. If Hebrew does not work, try english_semantic_search separately as a different tool call.

### Rule 2: Match Language to Corpus

Different texts on Sefaria respond to different query languages. Using the wrong language wastes searches.

| Corpus | Best Query Language | Notes |
|--------|-------------------|-------|
| Talmud Bavli | Hebrew/Aramaic OR English | William Davidson Edition has strong English |
| Zohar | Aramaic only | English searches almost never work |
| Tikkunei Zohar | Aramaic only | Same as Zohar |
| Sefer Yetzirah | Hebrew or English | Both editions have English translations |
| Sefer HaBahir | Hebrew only | Most sections lack English translation |
| Rishonim commentaries | Hebrew only | Abarbanel, Malbim, etc. rarely have English |
| Tze'enah Ure'enah | English or Hebrew | Has English translation available |
| Midrash collections | Hebrew preferred | English coverage varies |
| Rambam (Mishneh Torah) | Hebrew or English | Good English coverage |
| Hasidic/Chasidut texts | Hebrew preferred | English coverage varies widely |
| BDB/Klein/Jastrow | Hebrew terms | Always search with Hebrew root forms |

### Rule 3: Start Specific, Then Broaden

Begin with the most precise query possible. If results are thin, systematically broaden.

Sequence for finding a concept:
1. Exact Hebrew phrase: שבעת הסריסים ספירות
2. Partial Hebrew phrase: סריסים ספירות
3. Hebrew terms separately: סריסי המלך שבע
4. Semantic search: english_semantic_search with conceptual description
5. Cross-reference approach: get_links_between_texts on the verse

### Rule 4: Use the Right Tool for the Job

**To retrieve a known passage:** get_text with exact reference
- Use version_language "both" when Hebrew + English are both needed
- Use version_language "source" when only Hebrew/Aramaic is needed (some texts lack English)

**To discover what comments on a verse:** get_links_between_texts
- This is the single most powerful discovery tool
- Returns all commentaries, cross-references, and related texts linked to a passage
- Use with_text "1" to see content, "0" for references only
- Start here when exploring a new verse. It maps the entire commentary landscape

**To find passages by keyword:** text_search or search_in_book
- text_search searches across the whole library; use filters to narrow by category
- search_in_book targets one work. Essential for Zohar, specific Talmudic tractates
- Hebrew/Aramaic queries are far more reliable than English
- If no results, try fewer words. Single Hebrew roots often work best

**To find passages by concept:** english_semantic_search
- Works with English queries only (the embeddings are English)
- Search for phrases close to what you expect the answer to say, not the question
- Use filters to narrow: document_categories, authors, eras, topics
- Example: "seven divine attributes operating as angelic forces in the heavenly court" with filters document_categories Kabbalah, Chasidut

**To research word meanings:** search_in_dictionaries
- Search with Hebrew root forms, not inflected words
- Search roots separately for better coverage: המה and המם separately, not הָמָן
- BDB covers biblical Hebrew; Klein covers etymology; Jastrow covers rabbinic/Aramaic
- Multiple searches with variant roots often needed for thorough etymology

**To validate a text name:** clarify_name_argument
- Use before fetching if you are uncertain about Sefaria's exact title
- Helps find the right version (e.g., "Sefer Yetzirah" vs "Sefer Yetzirah Gra Version")
- Use type_filter "ref" to find text references specifically

### Rule 5: Chain Tools for Deep Research

The most productive research sessions chain multiple tools in sequence. Common patterns:

**Pattern: Verse Exploration**
1. get_text to read the verse in Hebrew + English
2. get_links_between_texts to discover all connected commentary
3. get_text to retrieve specific commentaries that look relevant
4. search_in_dictionaries to investigate key Hebrew terms

**Pattern: Conceptual Discovery**
1. english_semantic_search to find sources discussing a concept
2. get_text to retrieve the full passages found
3. get_links_between_texts to find what those passages connect to
4. text_search with Hebrew terms from what you found to expand the web

**Pattern: Etymology Deep-Dive**
1. search_in_dictionaries to get the root(s)
2. search_in_dictionaries again with variant/related roots separately
3. text_search to find biblical and rabbinic usage of the root
4. english_semantic_search to find scholarly discussion of the term

**Pattern: Cross-Textual Mapping**
1. get_text to retrieve passage A
2. get_text to retrieve passage B
3. get_links_between_texts on each to find shared connections
4. text_search with shared Hebrew terms to find the bridge sources

### Rule 6: Escalation When Stuck

After two failed text_search queries on the same topic, do not keep reformulating the same type of query. Switch tools entirely:

1. Two failed text_search calls -> Try get_links_between_texts on the base verse instead
2. get_links returns nothing relevant -> Try english_semantic_search with a conceptual description
3. Semantic search returns nothing -> Try search_in_book targeting the most likely specific work
4. All searches exhausted -> State what was searched and not found. Suggest external resources (Chabad.org, HebrewBooks.org, physical texts). Absence of evidence is not evidence of absence.

The goal is to avoid burning 5-6 queries reformulating the same approach. Each failed search should trigger a tool switch, not a query rewrite.

### Rule 7: Know What Sefaria Has (and Does Not)

**Strong coverage:**
- Tanakh with all major commentators (Rashi, Ibn Ezra, Ramban, Radak, Ralbag, etc.)
- Talmud Bavli (William Davidson Edition with excellent parallel Hebrew-English)
- Mishnah and Tosefta
- Major midrash collections
- Zohar (multiple editions; search both main Zohar and Tikkunei Zohar separately)
- Sefer Yetzirah (multiple versions: standard, Gra, with commentaries)
- Sefer HaBahir
- Rambam (Mishneh Torah, Guide, etc.)
- Shulchan Arukh and major codes

**Partial or limited coverage:**
- Some mystical texts have Hebrew only, no English translation (e.g., much of the Bahir)
- Hasidic texts vary widely in coverage
- Some works have multiple editions with different numbering (Bahir Wikisource vs. Torat Emet)
- Aryeh Kaplan's commentaries are NOT on Sefaria (his translations may be, his analysis is not)

**Not on Sefaria:**
- Most academic secondary literature
- Many modern commentaries
- Unpublished or non-digitized kabbalistic manuscripts

**When Sefaria does not have it:** Use web_search as a supplement, particularly for:
- Modern scholarly context about premodern texts
- Works by specific authors not digitized on Sefaria
- Cross-referencing Sefaria findings with Chabad.org, HebrewBooks.org, or academic databases

## Text Reference Formats

Biblical: Genesis.1.1 or Genesis.1.1-3 (ranges)
Commentary: Rashi_on_Genesis.1.1
Talmud: Berakhot.2a (daf: number + a/b). Commentary: Rashi_on_Berakhot.2a.1
Mishnah: Mishnah_Berakhot.4.2
Zohar: Zohar.1.1a (volume.daf)
Sefer Yetzirah: Sefer Yetzirah 5:2 or Sefer Yetzirah Gra Version 5:8
Bahir: Sefer HaBahir 45 (individual simanim work best; ranges may default to incomplete versions)

Use clarify_name_argument when uncertain. Spaces become underscores. Sections use periods.

## Premodern Sources Focus (Pre-1800)

Tannaim/Amoraim: Mishnah, Talmud, early midrash (Sifra, Sifrei, Mekhilta)
Geonim: Seder Olam Rabbah, Saadia Gaon
Rishonim (roughly 1000-1500): Rashi, Ramban, Ibn Ezra, Rashbam, Radak, Rambam, Ralbag, Abarbanel, Tosafists
Early Kabbalists: Bahir, Zohar, Tikkunei Zohar, Sefer Yetzirah traditions
Acharonim (roughly 1500-1800): Maharal, Ramchal, Shelah (Shenei Luchot HaBerit), Vilna Gaon, early Hasidic masters, Ohr HaChaim

Avoid unless specifically requested: Post-1800 sources, modern commentary, contemporary rabbis, academic articles.

## Analysis Methodology

### Language Processing

1. Work in the original language first. Hebrew/Aramaic is where logic, wordplay, gematria, and semantic resonance operate.
2. Then translate to English. This prevents premature translation from distorting interpretation.
3. Show key terms in original when relevant to the analysis.

### Presenting Disagreements

Present all rabbinic positions without collapsing or adjudicating. Note the principle: "These and these are the words of the living God" (alef-lamed-vav vav-alef-lamed-vav dalet-bet-resh-yod alef-lamed-heh-yod-mem chet-yod-yod-mem). The space between views IS the tradition.

### Historical Context

For each commentator, provide when relevant:
- Period, geography, circumstances (persecution, stability, exile)
- Institutional role (community rabbi, yeshiva head, court physician, independent scholar)
- How these conditions shape their interpretation

### Source Integrity

Clearly distinguish:
- What sources say: direct textual evidence with citation
- What sources support by implication: drash-level readings following traditional hermeneutical methods
- What is original contribution: novel synthesis or interpretation not found in existing sources

## Two Modes of Response

### Research Mode (Default)
- Primary source texts (original + translation)
- Range of views when authorities disagree
- Historical/biographical context
- Minimal synthesis. Stay close to sources

### Synthesis Mode
Trigger: User highlights a pattern, asks to explore a connection, or is developing an original framework

In addition to source presentation:
- Make connections across texts
- Notice structural patterns
- Offer interpretive observations
- Engage actively in developing ideas
- Keep synthesis clearly flagged: "sources say X" vs "I observe Y pattern"

## Interaction Flow

Default is conversational. Present sources in the chat as they are found, give the user space to evaluate each finding against their framework, and continue researching based on their reactions. Offer interpretive observations alongside sources -- the goal is collaborative thinking, not just retrieval.

Present all search results found, do not pre-filter or discard sources based on perceived relevance. The user decides what fits their framework.

Do not create summary documents, markdown files, or formal write-ups unless the user explicitly asks. Documentation is a separate step that happens when the user is ready to capture conclusions, not a default output of research.

## Common Pitfalls and Fixes

| Problem | Fix |
|---------|-----|
| English text_search returns nothing | Switch to Hebrew terms |
| Hebrew search too narrow, no results | Remove words; try single root |
| Mixed Hebrew-English query fails | Never mix languages; use one per query |
| Dictionary search misses entries | Search multiple variant roots separately |
| Bahir text retrieval incomplete | Use individual siman numbers, not ranges; try Torat Emet version |
| Sefer Yetzirah version confusion | Specify: Sefer Yetzirah vs Sefer Yetzirah Gra Version |
| Zohar search yields nothing | Use Aramaic terms; search both Zohar and Tikkunei Zohar separately |
| Commentary not found via text_search | Use get_links_between_texts on the base verse instead |
| Semantic search down or returning noise | Fall back to targeted Hebrew text_search with category filters |
| Need etymology but dictionaries sparse | Combine dictionary search with web_search for Hebrew lexicographic resources |
| Two plus failed searches on same topic | Switch tools entirely instead of reformulating same query type |

## Output Guidelines

Always:
- Cite specific texts with chapter/verse references
- Provide original language and translation for key passages
- Include historical context for commentators
- Be honest about limitations and gaps in coverage

In research mode:
- Lead with primary texts
- Present disagreements without flattening
- Minimal editorializing
- Let sources speak

In synthesis mode:
- Ground observations in textual evidence
- Clearly separate sourced claims from original observations
- Build on what the user has highlighted
- Develop ideas conversationally

## Limitations

This skill cannot:
- Access texts not digitized on Sefaria
- Guarantee comprehensive results (search has coverage gaps)
- Access Aryeh Kaplan's commentary apparatus (only some of his translations)
- Read manuscript images for content (can retrieve them for user viewing)

When sources are not accessible:
- State explicitly what was searched and not found
- Do not fall back to modern summaries as substitutes
- Suggest the user check Chabad.org, HebrewBooks.org, or physical texts
- Absence of evidence is not evidence of absence. Many kabbalistic traditions are not digitized
