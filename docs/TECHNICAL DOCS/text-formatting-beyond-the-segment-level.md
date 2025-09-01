---
title: Formatting within Sefaria Texts
excerpt: >-
  HTML supported by Sefaria for in-line text formatting, footnotes,
  commentaries, and other emphasis.
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
      slug: text-references
      title: Text References
---
On occasion, it becomes necessary to convey information about a specific word or location within a segment. A common example is text formatting. The structure we outlined in [The Structure of a Book on Sefaria](doc:the-structure-of-a-text-on-sefaria) lacks a built-in method for indicating that certain words require distinct formatting.

A common solution is to use html/xml tags within a segment. A major drawback of this is that the data within these tags is stored in the specific version of the text, as opposed to on the index, which provides a universal structure for all versions of the text. Unfortunately, this is the best method we currently have for including sub-segment level data.

## Text Formatting

Formatting text is relatively straightforward. One must only wrap the text that is to be formatted in an appropriate html tag. So, if one wishes to mark a word as bold, he or she would simply wrap it in a `<b>` tag. This is commonly used in commentaries that start off with a quote from the text they are commenting on (דיבור המתחיל).

## Footnotes

Footnotes contain two parts - a marker (usually a number) and text. Footnotes can be added to text using a `<sup class="footnote-marker">` tag for the marker and an `<i class="footnote">` tag for the text. An example of a segment with a footnote:

`The main text is here <sup class="footnote-marker">1</sup><i class="footnote">The text inside the footnote</i>and the main text continues here.`

For an example of a footnote on Sefaria, see the asterisk in the first verse of Genesis in [this version](https://www.sefaria.org/Genesis.1.1?ven=The_Contemporary_Torah,_Jewish_Publication_Society,_2006&lang=bi&aliyot=0). 

## Inline Reference

Sometimes a commentary will refer to a specific word or phrase within another text, but will not quote such a text. In printed books, a marker similar to a footnote can be found. Placing an entire commentary inside the parent text as we did with footnotes is not acceptable. In such a case, we display a small marker within the segment that will become apparent to the user when a commentary that requires such a reference is selected in the sidebar.

Creating such a reference requires data to exist not only within the text segment, but also on the link between the commentator and the parent text. The parent segment will contain an `<i>` tag with the css properties `data-commentator` and `data-order`. The `data-commentator` field must match the `collective_title` of the text (i.e. so for `Rashi on Genesis`, the `data-commentator` would match the `collective_title` of `Rashi`), and `data-order` will usually match the segment number of the linked comment (the exception to this rule is when individual comments are so long that they have their own internal structure - in such cases the section number can be used). In addition, an optional `data-label` can be added. This is an all purpose override of any display logic - whatever `data-label` is set to is what will be displayed.

The link object requires the field `inline_reference`. The value of `inline_reference` will be a dictionary with keys `data-commentator`, `data-order` and `data-label`(optional) whose values will be identical to what was set in the `<i>` tag in the parent segment.

Parent segment:  
`Lorem ipsum <i data-commentator="Child" data-order="1"></i>consectetur adipiscing elit.`

Link Object:

```
{
    'refs': ['Parent 1:1', 'Child 1:1'],
    'type': 'commentary',
    'inline_reference': {
        'data-commentator': "Child",
        'data-order': 1
    }
}
```

Here as well, the `data-commentator` field must match the `collective_title` of the commentator.

## Allowed HTML Tags and Attributes

Below is a chart of all of the allowed HTML tags in Sefaria texts, with their corresponding attributes (if any exist) and how they are used within the library:

### Allowed HTML Tags with Corresponding Attributes

The following is a list of allowed HTML tags that have associated attributes. The table provides insight both into the tags and attributes supported by Sefaria, and how we specifically use them within texts in our library.

[block:parameters]
{
  "data": {
    "h-0": "Tag",
    "h-1": "Attributes",
    "h-2": "Use in Sefaria Texts",
    "0-0": "`<i>`",
    "0-1": "`class`  \n  \n`dir`  \n  \n`data-overlay`  \n  \n`data-value`  \n  \n`data-commentator`  \n  \n`data-order`  \n  \n`data-label`",
    "0-2": "There are three uses of `<i>` tags:  \n1. **Footnotes**: Uses content internal to the`<i>` tag as explained above.  \n2. **Commentary Placement**: Uses the `data-commentator`, `data-order` and `data-label` attributes, also explained in the previous section.  \n3. **Structure Placement**: This indicates page transitions. Uses the `data-overlay` and `data-value` attributes.  \n  \nThe `class` attribute is used for CSS styling, as is standard.  \n  \nThe `dir` attribute is used for indicated text direction inline, so for example `rtl` for a text that is right-to-left, or `ltr` for a text that is left-to-right. ",
    "1-0": "`<img>`",
    "1-1": "`src`  \n  \n`alt`",
    "1-2": "This tag is used for images in text, for example as seen in [Mishnat Eretz Yisrael](https://www.sefaria.org/Mishnat_Eretz_Yisrael_on_Pirkei_Avot.1.1.42?lang=en&with=all&lang2=en).  \n  \n`src` is the URL to the image.  \n  \n`alt` is an alt-text for the image.",
    "2-0": "`<sup>`",
    "2-1": "`class`",
    "2-2": "This tag is used for footnotes. For more on footnotes see the section above. ",
    "3-0": "`<span>`",
    "3-1": "`class`  \n  \n`dir`",
    "3-2": "This tag is used inline for text when specific CSS styling (or direction) needs to be applied to a specific sub-segment of the text. ",
    "4-0": "`<a>`",
    "4-1": "`dir`  \n  \n`class`  \n  \n`href`  \n  \n`data-ref`  \n  \n`data-ven`  \n  \n`data-vhe`  \n  \n`data-scroll-link`",
    "4-2": "This tag is used for links featured within a version of the text. The Sefaria-specific attributes will be detailed below:  \n  \n`data-ref`- A way to refer to the `Ref` of the given text inline  \n`data-ven` - A way to refer to the English version of a text inline  \n`data-vhe` - A way to refer to the Hebrew version of a text inline  \n`data-scroll-link`- A way to contain a link within the same book, and upon clicking, scroll to the new segment of the text instead of opening the text in a new panel."
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


### Allowed HTML Tags

The following tags are allowed on Sefaria, and do not have any supported attributes associated with them. 

| Tag        | Use in Sefaria Texts                                                                                                                                                                                                                                      |
| :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<b>`      | This tag is used to bold a segment of the text, such as in the above example of emphasizing a Dibbur HaMatchil.                                                                                                                                           |
| `<br>`     | Line breaks in a text. Sefaria does not support use of `<p>` within a version of a text.                                                                                                                                                                  |
| `<strong>` | Another way of bolding portions of a text, present in some versions.                                                                                                                                                                                      |
| `<em>`     | Another way of italicizing portions of a text, present in some versions.                                                                                                                                                                                  |
| `<sub>`    | Subscripts.                                                                                                                                                                                                                                               |
| `<u>`      | An outdated tag for underlining a portion of the text. Modern web development encourages the use of CSS for all text styling. This tag is supported, since it might be present in old digitized versions of Jewish texts inherited by Sefaria.            |
| `<big>`    | An outdated tag for increasing the size of a portion of the text. Modern web development encourages the use of CSS for all text styling. This tag is supported, since it might be present in old digitized versions of Jewish texts inherited by Sefaria. |
| `<small>`  | An outdated tag for decreasing the size of a portion of the text. Modern web development encourages the use of CSS for all text styling. This tag is supported, since it might be present in old digitized versions of Jewish texts inherited by Sefaria. |