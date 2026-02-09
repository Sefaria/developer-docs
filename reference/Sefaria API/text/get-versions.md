---
title: Versions
excerpt: >-
  The Versions API uses the title of a valid Sefaria Library `index` as a
  parameter and returns all available versions of the indicated text in the
  Sefaria database along with metadata for each version. A version may be a
  translation of the specific text, an alternative language, or any other text
  associated with that index. _Please note:_ In order to receive the desired
  results, please ensure you are passing a recognized `index`. For example:
  `Rashi on Genesis` is a valid `index` while `Rashi` is not. To see a complete
  list of valid Sefaria indices, click
  [here](https://www.sefaria.org/api/index/)
api:
  file: sefaria-api.json
  operationId: get-versions
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---