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

<br />
