---
title: The Sefaria Developer Community Archive
deprecated: false
hidden: false
metadata:
  robots: index
---
Sefaria hosts a Developer Community on Discord where folks can learn, build and collaborate on everything at the intersection of Torah and technology.&#x20;

[Join us inside!](https://sefaria.formstack.com/forms/sefaria_developer_discord_community)

***

# Archive

## August 7th, 2026: Weekly Review

### :sparkles:Notable Projects & Community Tools

**Sefaria Web Components Project Launch**&#x20;

A community developer employed at Microsoft announced an incubating project for Microsoft's Global Hackathon 2026: **Sefaria Web Components** — reusable, composable web components (e.g., `, `) that abstract common functionality like punctuation toggling, BIDI support, and link navigation.&#x20;

- **Key Value Proposition:** Eliminate wheel-reinvention across Sefaria API implementations by providing standardized, extracted components from existing web and mobile codebases, plus utility libraries like `@sefaria/ref`.&#x20;
- **Call for Feedback:** The team is actively seeking input on pain points and desired features. If you're building with the Sefaria API, this is a good time to share what standardized components would solve for you.&#x20;
- **Repository:** [https://github.com/Arithmomaniac/sefaria-web-components](https://github.com/Arithmomaniac/sefaria-web-components)&#x20;

### :handshake: Community Connection&#x20;

A developer inquired about connecting with the creator of vehagita.co.il (a Hebrew social network for Torah study). If you're the developer behind this project or know them, they're looking to connect in the community.

## July 31st, 2026: Weekly Review

### :sparkles:Notable Projects & Community Tools

- **Sefarim.net Launch**: Creator shared a semantic search project built on Sefaria's API, enabling multilingual Tanakh Q\&A with Claude-powered citation verification (English, French, Hebrew). The project demonstrates preference for use of [Sefaria-Export](https://github.com/Sefaria/Sefaria-Export) instead of API batch requests. Two new projects were added to the Powered-by-Sefaria page this week.&#x20;
- **Terminal-Based Kabbalah Tools**: Developer inquired about contributing Hebrew dictionary and gematria/cipher mapping applications to Sefaria.&#x20;

### :wrench:Key Technical Insights&#x20;

- **Patot Library Issues Resolved**: A developer identified and reported critical dependency problems in Patot's chunking module (undeclared `stanza` dependency, hardcoded machine-specific paths). The Sefaria Research Team confirmed these were fixed in v0.1.5 but the README still referenced older installation instructions—now updated.&#x20;
- **Disambiguator Project Impact**: The Sefaria Research Team clarified that the new Disambiguator project resolves ambiguous citations (like "ibid" references) to segment-level precision, making previously hidden cross-references visible in the UI. Future work may include detecting quotations without explicit citations using trained models rather than LLMs (cost/scale considerations).&#x20;

### 🎯Support & Best Practices&#x20;

- **Data Access Guidance**: The team consistently directed developers to use the [Sefaria-Export](https://github.com/Sefaria/Sefaria-Export) for bulk data needs rather than the API, which is simpler and more efficient than sequential API calls.&#x20;
- **Communication Culture**: Team encouraged public channel discussions over DMs to benefit the broader community.

## July 24th, 2026: Weekly Review

### :sparkles:Notable Projects & Community Tools&#x20;

#### **New Powered-by-Sefaria Projects:**&#x20;

- [**Yochai**](https://www.yochai.wiki/) ([Lightning Studios](https://www.lightningstudios.ai/)): A Socratic AI chevruta leveraging 1,000+ primary sources and 16M entity relationships&#x20;
- [**AI Torah**](https://aitorah.ai/): Instant answers on Jewish law and life using Sefaria's library&#x20;
- **Jastrow Dictionary Tool**: A game-changing Gemara learning tool enabling word lookup without prior lemmatization. The developer is crowdsourcing accuracy reviews at [https://daniepstein.com/gold-review/](https://daniepstein.com/gold-review/) and plans open-source release. The reasoning behind the development of a tool like this can be found [here](https://daniepstein.com/gold-review/why-not-just-sefaria.html).&#x20;
- [**Sefaria Links**](https://zakdev26.github.io/sefaria-links/): User-friendly tool for building source sheets, exporting to Kindle/Word/PDF with AI integration support&#x20;

### :wrench:Infrastructure & API Insights&#x20;

**High-Volume API Usage Alert**: The Sefaria team identified several high-volume automated clients and offered optimization alternatives:&#x20;

- Supabase Edge Functions: \~800k requests/day (33% erroring)&#x20;
- Google Apps Script: \~60k requests/day on `/api/sheets` endpoint&#x20;
- Node service: \~200k requests/day on `/api/search-wrapper/es8`&#x20;
- Bulk text corpus downloads: 100k-300k requests/day from various projects&#x20;

**Recommendation**: For bulk data needs, use [Sefaria-Export](http://github.com/Sefaria/Sefaria-Export) (structured JSON exports) instead of per-ref API calls—faster for developers, lighter on infrastructure.&#x20;

### 🎯 Support Questions & Answers

**Q: 504 errors on link endpoint for multiple verses**&#x20;

A: Use Sefaria-Export for bulk data; consider the [Sefaria Developers MCP](https://developers.sefaria.org/docs/the-sefaria-mcp) to optimize API calls.&#x20;

\-

**Q: Available language translations?**&#x20;

A: Sefaria has texts across 22 different languages. To learn more, see the API Reference: [get-translations endpoint](https://developers.sefaria.org/reference/get-translations)&#x20;

\-

**Q: Jastrow dictionary downloads for offline use?**&#x20;

A: Not in standard Sefaria-Export, but community member created [this exporter](https://gist.github.com/Arithmomaniac/924ef9e00ff2cabf72142d75c9263da0) (Mongo to JSONL/CSV)&#x20;

<br />

**Reminder:**

In addition to the Sefaria MCP, there's a separate MCP available for developers.sefaria.org to help your agent gain fluency in our docs and API. Learn more [here](https://developers.sefaria.org/docs/the-sefaria-mcp).&#x20;

<br />

**Policy Note&#x20;**

Sefaria maintains a community-first integration approach—excellent tools stay independent in the "[Powered by Sefaria](https://developers.sefaria.org/docs/powered-by-sefaria)" registry rather than direct adoption, balancing ecosystem growth with user autonomy.

## July 16th, 2026: Weekly Review

### &#x20;🎯 Key Support Questions & Answers

- **Source Sheet Creation via API** - A developer asked about creating sheets programmatically. The team clarified that POST The `api/sheets` endpoint is undocumented and discouraged because it requires developers to supply full text payloads. Instead, authenticated cookie-based requests are recommended for updating existing sheets, with a provided Python script template. However, this remains a friction point: developers want to reference Sefaria refs without pre-fetching text.
- **Text Update Tracking** - No dedicated API exists for checking if texts have been updated since a given date. The team pointed to workaround: `https://www.sefaria.org/activity/{index_name}/{lang}/{version}` accessible via GUI revision history. A developer requested a proper API for this (useful for offline readers like Stndr that need update checks). This appears to be a feature request worth considering.&#x20;
- **Data Dump Frequency** - The small MongoDB dump (`dump_small.tar.gz`) updates every 24 hours but lags live changes. Note it excludes edit history, private collections, and copyright-restricted texts.&#x20;

### 💡 Notable Insights

- **Language Code Inconsistency**: The team acknowledged a legacy issue where activity URLs use `en`/`he` codes regardless of actual language (ISO codes like the database's `actualLanguage` field would be preferable). A known limitation. &#x20;
- **Agent Skills for Source Sheets**: Proposal to add source sheet editing as an AI agent capability for power users—interesting direction for automation.&#x20;

## **July 9th, 2026: Weekly Review**

### :wrench: Project Highlights

- **IvritSuite**: Hebrew learning suite with worksheet generator, dictionary, Torah Trainer, and custom font creator&#x20;
- **Ruth**: Hebrew learning app targeting conversion-curious learners with TTS pronunciation&#x20;
- Idea for an app for Gemara Aramaic fluency using game-based learning and story connections rather than pure translation&#x20;
- **Jastrow\.app**: Making the Jastrow Dictionary more accessible&#x20;
- **SeferAI.org**: AI-powered shiurim generation using Claude + text-to-voice; seeking developers to scale
- **Derekh Learning**: AI-generated digestible lessons with multiple narrative voices; available as iOS app with Android port in progress
- **Rabbinic Reasoning Lab**: Interactive learning for Talmud and Midrash, trained on Pardes materials and academic research
- **Stndr**: Desktop UI for Sefaria with offline/online hybrid functionality and planned collaborative study features&#x20;
- **Sefaria Discord Bot**: French community tool supporting Bible questions in 3 languages; successfully tested with Chidon HaTanakh questions

### 💡 Technical Insights&#x20;

- **TTS Challenges**: Developers discussed training a custom Hebrew model with 11Labs due to lack of native Hebrew speech; now exploring Gemini TTS alternatives&#x20;
- **Data Opportunities**: Chidon HaTanakh questions identified as valuable sample dataset for testing Jewish text chatbots&#x20;

### &#x20;🤝 Collaboration Interests&#x20;

- Developer seeking developer partner for comprehensive Midrash app&#x20;
- **Multiple developers** expressing openness to partnerships and collaboration on overlapping projects&#x20;
