---
title: Terms
excerpt: >-
  An object for grouping titles, alternate titles and other bilingual terms
  reused across Sefaria.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: text-formatting-beyond-the-segment-level
      title: Text Formatting Beyond the Segment Level
---
## Terms

A `Term` is a `sharedTitle `node which is used to group titles, alternate titles and titles in other languages (i.e. Hebrew).

Some objects, such as `Index` records, store their own various titles. However, for other system objects, that is not currently a possibility. To this end, we use the `Term` object to add titles to items that need them, such as categories, `sharedTitles` on [Categories](doc:categories), and section names.

Here's a sample `Term` for the section name for `chapters`:

```json
{
  "scheme" : "section_names",
  "titles" : [
    {
      "lang" : "en",
      "text" : "Chapters",
      "primary" : true
    },
    {
      "lang" : "he",
      "text" : "פרקים",
      "primary" : true
    }
  ],
  "name" : "Chapters"
}
```

And here's a `Term` for the table of contents sub-category of `Geonim`:

```json
{
  "name" : "Geonim",
  "titles" : [
    {
      "lang" : "en",
      "text" : "Geonim",
      "primary" : true
    },
    {
      "lang" : "he",
      "text" : "גאונים",
      "primary" : true
    }
  ],
  "scheme" : "toc_categories"
}
```