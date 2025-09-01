---
title: The Structure of a Book on Sefaria
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Each book on Sefaria is represented by a unique Index, which may have one or
    more Version objects (translations or editions) associated with it, and is
    supported by a schema that provides structural and metadata information.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: each-book-is-an-index
      title: Index and Versions
---
Each book on Sefaria is represented by an `Index`. Each book has one, and only one, `Index` associated with it.

Each version of a text (a translation, a different edition, etc.) is represented by a `Version` object. A book will have a single `Index` associated with it.  An Index could have no `Version` objects associated, but it will generally have one or more `Version` objects associated with it.

For a deep dive on `Index` and `Version`, see [Index and Versions](doc:each-book-is-an-index). 

Each `Index` is supported by a schema to represent the structure of that book, and provide important metadata and structural information regarding the book. To take a deep dive into this structure, read about [the Index Schema](doc:the-index-schema).
