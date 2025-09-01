---
title: Getting Started With The Sefaria API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: endpoint
      slug: tutorial-dvar-torah-outliner
      title: 'Tutorial: Dvar Torah Outliner'
---
## The Sefaria API

The Sefaria API allows live access to Sefaria's structured database of Jewish texts and their interconnections. It is designed to make getting a new web or mobile app up-and-running as simple as possible. 

## Welcome to the Sefaria API Reference.

Here you will find documentation and interactive playgrounds for many of our most important API endpoints. Our reference is powered by our OpenAPI spec, which you can see [here](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json). 

> 🚧 Work-In-Progress
> 
> Please note, this reference is a work-in-progress. We are always striving to refine and improve our API itself, as well as our documentation. We are looking forward to documenting additional endpoints here in the future. Looking for something you can't find? [Contact us](page:contact-us), we'd love to hear from you.

For additional documentation, check out our technical docs [here](https://developers.sefaria.org/docs/welcome).

## Getting Started

There isn't much needed to get started with our API; all of our currently documented endpoints can be reached without needing any API keys, tokens or authorization. 

**Need a Simple Example?** In this [tutorial](ref:tutorial-dvar-torah-outliner), we guide you step-by-step through a very simple script that uses a handful of our endpoints. 

**Looking to Dive Deep?** You can jump right into our API Reference [here](ref:get-v3-texts)

**Still Puzzled?** - You can see what [others have asked](ref:others-have-asked) or [contact us](page:contact-us) for help! 

## API Pathways

Below are a few different pathways through the Sefaria API for individuals seeking to acquaint themselves with specific aspects of our data. Organized by use case, these entry points can assist you in navigating to the data you need as you're learning the API.

### Where are the books?

At its core, Sefaria is an open-source digital library of Jewish books. To see all of the books available right now on Sefaria, see the [Table of Contents](ref:get-index). To retrieve all of the metadata related to a specific book, try out the [Index (v2)](ref:get-v2-index) endpoint. 

### Ready to dive into some text data?

Start with [Texts (v3)](ref:get-v3-texts) to retrieve text editions, along with all of the metadata for that given version. Dive into the [Versions](ref:get-versions) endpoint to see all available editions of a given book. 

### Links, Links, Links

One of the most powerful aspects of Sefaria's data is the links, and other relations, between various texts. To get started with seeing all of the links, check out our very powerful [Related](ref:get-related) API. This is your ticket to retrieve related commentaries on a verse, parallel passages, and more.

### Trying to build something based on a learning schedule?

Explore our [Calendars](ref:calendars) API to find data about all of the different study schedules tracked on Sefaria. 

### Topics

Interested in our Sefaria curation of Topics, and the associated texts? Check out the [Topic Graph](ref:get-topics-graph) endpoint to retrieve interrelated topics, and the [Ref-Topic-Links](ref:get-ref-topic-links) endpoint to retrieve all of the topics associated with a given text. 

### Interested in Images?

Use our [Social Media Image](ref:get-img-gen) endpoint to generate nice graphics based on a segment of text of your choosing. 

Alternatively, if you are looking for a manuscript image, see the [Manuscripts](ref:get-manuscripts) endpoint to retrieve correlating manuscript pages for a given text. 

### Need a dictionary?

Check out the [Lexicon](ref:get-words) endpoint and [Word Completion](ref:get-words-completion) endpoint to get started.

### Building something for non-English speakers?

Use our [Languages](ref:get-translations) API to see all of the various languages on Sefaria, and query [Translations](ref:get-translations-lang) to see all text editions available for that language. Then, head over to the [Texts (v3)](ref:get-v3-texts) to query the specific text you need.

### Random

Need a random text? See [Random Text](ref:get-texts-random) and [Random By Topic](ref:get-random-by-topic) endpoints.