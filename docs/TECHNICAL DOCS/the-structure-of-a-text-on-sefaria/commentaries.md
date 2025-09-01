---
title: Commentaries
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
    - type: basic
      slug: terms
      title: Terms
---
# How Sefaria Treats Commentaries

In the Sefaria database, commentaries are treated the same as regular texts. This means that commentary texts have their own individual `Index` and each edition of text for that commentary is represented by its own `Version` record.

## Index Attributes for Commentaries

We use specific attributes on the `Index` record to indicate that an `Index` is a commentary on another book. Those attributes are elaborated below:

[block:parameters]
{
  "data": {
    "h-0": "Attribute",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`dependence`",
    "0-1": "`String`",
    "0-2": "Generally `Commentary` or `Targum`. This field is used to denote commentaries and other potential non-standalone texts.",
    "1-0": "`base_text_titles`",
    "1-1": "`List` of `Strings`",
    "1-2": "The base book(s) this one is dependent on. For example, `Rashi on Genesis` is dependent on `Genesis`.",
    "2-0": "`base_text_mapping`",
    "2-1": "`String`",
    "2-2": "Enables auto-linking between a base text and a commentary. These values  match keys in the `class_map` found in  [sefaria.helper.link.AutoLinkerFactory](https://github.com/Sefaria/Sefaria-Project/blob/fe9c971217b99cd763c0dd8f42b82339379b961c/sefaria/helper/link.py#L325).  \n  \nSo far, four options exist: `\"one_to_one\"`, `\"many_to_one\"`, `\"many_to_one_default_only\"` and `one_to_one_default_only`.  \n  \n`many_to_one` - The typical case of multiple commentary comments on a single segment of the base text. For example, there are many Rashi comments per verse of Tanakh. Tanakh is a depth-2 text, and Rashi on Tanakh is depth-3.  \n  \n`one_to_one` - Only used for commentaries defined as `targum`. They are linked to their original texts one-to-one since they are structured exactly the same as their base text - both Targum on Amos and Amos are depth-2 (a second example would be Steinsaltz on Bavli, the commentary has the same depth as the text, and also functions as a `one-to-one` commentary).  \n  \nThe corresponding `default` keys work the same, except `default` nodes will be linked and other nodes will be ignored.  \n  \n(This helps maintain the auto-linking feature once afforded commentaries by default. It also makes room to expand and have various algorithms for linking between a dependent text and its base text/s)",
    "3-0": "`collective_title`",
    "3-1": "`String`",
    "3-2": "This is the value for a group of index records. It is used to contain the former commentator name which was once the primary handle on a commentary. Now used to group many indices belonging to a larger corpus, such as commentaries.  \n  \nRequires a matching Hebrew `Term`.  \n  \nex: The `collective_title` for `Rashi on Genesis` is the same as the `Index` title, `Rashi`. ",
    "4-0": "`is_cited`",
    "4-1": "`Boolean`",
    "4-2": " Only texts with `is_cited` set to `True` will be picked up as a citation in referencing texts.  \n  \nDefault is `True`.  "
  },
  "cols": 3,
  "rows": 5,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


Below is an example of how these specific fields would be filled on the `Index` record of `Ramban on Genesis`:

```
"title": "Ramban on Genesis"
"base_text_titles": [
   "Genesis"
],
"base_text_mapping": "many_to_one",
"collective_title": "Ramban",
"dependence": "Commentary",
"categories": ["Tanakh", "Rishonim on Tanakh", "Ramban", "Torah"]  
```

## Commentaries and Category

[Categories](doc:categories) are mostly used to denote where the book will show up in the [Sefaria Table of Contents](https://sefaria.org).  
This means that the text `"Ramban on Genesis"` with the category path of  `["Tanakh", "Rishonim on Tanakh", "Ramban", "Torah"]`will show up under [Tanakh](https://www.sefaria.org/texts/Tanakh) >> Rishonim on Tanakh >> [Ramban](https://www.sefaria.org/texts/Tanakh/Rishonim%20on%20Tanakh/Ramban) >> Torah. 

- For a commentary, the category indicating the type of commentary (i.e. in this example, `Rishonim on Tanakh`) comes after one top level category . E.g.`["Tanakh", "Rishonim on Tanakh", "Ramban", "Torah"]`

- In some cases, it comes after multiple categories, particularly for Talmud. E.g.`["Talmud", "Bavli", "Rishonim on Talmud", "Rashi", "Seder Zeraim" ]`