---
title: Versions
excerpt: >-
  The versions API takes a title of a valid Sefaria `index` as a parameter, and
  will return all available versions of the text in the Sefaria database,
  alongside metadata for each version. A version can be a translation of the
  text, an alternative language, or any other text associated with that index. 


  *Note:* In order to see the expected results, you need to make sure you are
  passing a recognized `index`. For example, `Rashi on Genesis` is a valid
  `index` but `Rashi` is not. To see the entire list of valid Sefaria indices,
  click [here](https://www.sefaria.org/api/index/)
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