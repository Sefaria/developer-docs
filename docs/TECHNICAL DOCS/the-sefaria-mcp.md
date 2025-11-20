---
title: The Sefaria MCPs
excerpt: >-
  Use our MCPs to integrate your LLM of choice with Sefaria's rich library of
  Jewish texts and huge cache of open-source data.
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  robots: index
---
The Sefaria MCP (Model Context Protocol) servers connect your favorite AI tools directly to Sefaria's vast library of Jewish texts — and to our API and data. Writing code and answering questions just got a lot easier.

## What is an MCP?

An MCP is a standardized way for LLMs to connect to external data sources and APIs. It's a bridge that lets your AI chat with databases, search engines, and specialized libraries. Think of it as an adaptor cable that lets your AI tools connect seamlessly with an external source.

### The Sefaria Developers MCP [[https://developers.sefaria.org/mcp](https://developers.sefaria.org/mcp)]

This tool allows your AI assistant of choice to converse directly with the Sefaria API. This means your LLM can search and retrieve any data and write the code you need to build tools and programs based on Sefaria’s indices. Connect to Claude Code, Cursor, or any other MCP-Compatible AI tool, and watch the JavaScript write itself.

### The Sefaria Texts MCP [[https://mcp.sefaria.org/sse](https://mcp.sefaria.org/sse)]

This tool allows your AI assistant of choice to converse directly with the Sefaria library. This means your LLM can search and retrieve any text from our database and return a response with precise citations to specific sources. Now, when you ask Claude or ChatGPT what Rashi says about Genesis 1:6, you can be sure you’re getting a precise answer.

<Callout icon="❗️" theme="error">
  **Please note:** You must have a paid account to connect an MCP
</Callout>

## Quick Setup Guide

### For Any MCP-Compatible Tool:

1. Copy the relevant MCP URL:
   1. Sefaria Texts MCP [https://mcp.sefaria.org/sse](https://mcp.sefaria.org/sse)
   2. Sefaria Developers MCP [https://developers.sefaria.org/mcp](https://developers.sefaria.org/mcp)
2. Add to Your AI Tool:
   1. **Claude**: Go to settings, add as a Custom Connector, enable.
   2. **Cursor**: Configure in MCP settings
   3. **ChatGPT**: Go to settings, navigate to Apps and Connectors, click Create, configure accordingly
   4. **Other Tools:** Use the MCP URL in your client's server configuration
3. Start Exploring:
   1. Using the Sefaria Texts MCP, ask your questions about Jewish texts.
      For example:
      1. "What does Genesis 1:1 say?" or "Find me talmudic discussions about kindness"
   2. Using the Sefaria Developers MCP, ask the AI assistant to write code by pulling from our API.
      For example:
      1. “Help me build a GUI which uses the Sefaria API to focus on Sforno's commentary on Genesis.”