---
title: Category
excerpt: >-
  GET requests take a full category path in the request, e.g.
  `/api/category/Tanakh/Torah/Genesis`, and return the full category object
  found. 


  If the category is not found, the returned object will have an error
  attribute. If any element of the path is found, the API will return the
  closest parent in an attribute called `closest_parent`. This is useful for
  proactively looking up a category before posting an `Index` to it.
api:
  file: sefaria-api.json
  operationId: get-category
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---