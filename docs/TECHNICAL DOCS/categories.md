---
title: Categories
excerpt: Categorization of texts in the Sefaria library.
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
      slug: getting-started
      title: Getting Started With The Sefaria API
---
All texts at Sefaria are sorted into categories. When interacting with [Sefaria.org](https://sefaria.org), one immediately sees the fourteen highest level categories, and uses those to peruse the library. Categories are first-class objects at Sefaria, and can be retrieved via the Categories API. 

Categories are present on the `Index` of a text, and are a requirement for `Index` creation. For example, the `Index` of `Genesis`has a category of `Torah`. The `Torah` category is a child of the `Tanakh` highest level category, so the full category for `Genesis` would look like `["Tanakh", "Torah"]`. 

To learn more about the Sefaria `Index`, see [The Structure of a Book on Sefaria](doc:the-structure-of-a-text-on-sefaria) or [the Index API](ref:get_api-v2-raw-index-index-title). 

## Structure of the Category Object

### Required Fields:

#### `path`

Category objects require `path`, which is a list of primary titles of categories that form the path of ancestors to this one.  The current category's title is included in the path. The path is stored in a list, with the highest level ancestor at position 0, and the current category's title at the end of the list. 

| Category                                                                                                             | Full Path of the Category Object                                                                              | Depth | Highest  Ancestor | Current Category (`lastPath`) |
| :------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------ | :---- | :---------------- | :---------------------------- |
| [Tanakh](https://www.sefaria.org/texts/Tanakh)                                                                       | `["Tanakh"]`                                                                                                  | `1`   | `Tanakh`          | `Tanakh`                      |
| [Korban Netanel](https://www.sefaria.org/texts/Talmud/Bavli/Rishonim%20on%20Talmud/Rosh/Commentary/Korban%20Netanel) | `[     "Talmud",     "Bavli",     "Rishonim on Talmud",     "Rosh",     "Commentary",     "Korban Netanel" ]` | `6`   | `Talmud`          | `Korban Netanel`              |
| [Rashbam](https://www.sefaria.org/texts/Tanakh/Rishonim%20on%20Tanakh/Rashbam)                                       | `[    "Tanakh",      "Rishonim on Tanakh",      "Rashbam" ]`                                                  | `3`   | `Tanakh`          | `Rashbam`                     |

***

#### `titles` or `sharedTitle`

Categories also must contain either `titles` or `sharedTitle`:

- `titles` is a list of `title` objects, with each object containing the language code (`lang`), the `text`, and a boolean indicating whether or not this title is primary. An example of the titles list on the `Index` of `Bamidbar` can be found below: 

```
[
	{
		"lang" : "he",
		"text" : "במדבר",
		"primary" : true
  },
{
		"lang" : "en",
		"text" : "Numbers",
		"primary" : true
`}
]
```

- `sharedTitle`: a string key for an existing `Term` object. A `Term` is a shared title block which can be used across multiple `Index` objects and `Category` objects. **Note:** This Term must exist before attempting to create the category. 

### Derived Attributes:

#### `lastPath`

`lastPath` is derived from the `path` attribute, and doesn't need to be specified on object creation. This attribute contains the title of the current category, and is represented by the last string in the path list. 

#### `depth`

`depth` is derived from the `path` attribute, and doesn't need to be specified on object creation.

### Optional Attributes

- `enDesc`: An English language description of the category.
- `heDesc`: A Hebrew language description of the category. 
- `isPrimary`: A boolean flag.  When `True`, this category won't be nested within another, but will have its own page. An example of this is the Mishnah category, which you can see [here](https://sefaria.org/texts/Mishnah)
- `searchRoot`: A string.  When present, this category will be moved within the specified root category, in the context of search filters.

## Example Category Objects

### Example 1: The `Category` for the commentary of `Metzudat David`:

```text JSON
{
    "lastPath" : "Metzudat David",
    "path" : [
        "Tanakh",
        "Acharonim on Tanakh",
        "Metzudat David"
    ],
    "depth" : NumberInt(3),
    "enDesc" : "",
    "heDesc" : "",
    "enShortDesc" : "18th-century classic commentary on Prophets and Writings explaining verses in clear and simple language, primarily based on the Radak’s commentary.",
    "heShortDesc" : "פירוש קלאסי מן המאה ה-18 על נביאים וכתובים, המתבסס בעיקר על פירוש רד\"ק ומבאר את הפסוקים בשפה ברורה ופשוטה.",
    "sharedTitle" : "Metzudat David"
}
```

### Example 2: The `Category` for the commentary of `Targum`:

```text JSON
{
    "lastPath" : "Targum",
    "path" : [
        "Tanakh",
        "Targum"
    ],
    "depth" : NumberInt(2),
    "enDesc" : "Targum (“translation”) is the name of a category of texts that translate the Tanakh into Aramaic, originally transmitted orally and committed to writing between the 1st and 6th centuries CE.",
    "heDesc" : "הקטגוריה של 'תרגומים' כוללת את תרגומי התורה לארמית, תרגומים שהועברו במקור בעל פה והועלו על הכתב בין המאה ה-1 ל-6 לספירה.",
    "enShortDesc" : "Translations of Tanakh into Aramaic, originally transmitted orally and mostly committed to writing between the 1st and 8th centuries CE.",
    "heShortDesc" : "תרגום של התנ\"ך לארמית, במקור הועבר בעל פה, והועלה על הכתב מן המאה ה-1 עד המאה ה-8 לספירה.",
    "sharedTitle" : "Targum",
    "searchRoot" : "Targum",
    "order" : NumberInt(20)
}
```