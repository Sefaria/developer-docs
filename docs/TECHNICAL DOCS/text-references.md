---
title: Text References
excerpt: >-
  At the core of Sefaria's system is the system of text references.
  Understanding how they work is key to using our API, and our broader codebase.
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
      slug: topic-ontology
      title: Topic Ontology
---
# Text References (Citations)

Sefaria depends on the ability to unambiguously refer to particular segments of text in the repository. The implementation of text references in Sefaria has the following goals:

- References should be human and machine readable.
- References should be environment agnostic (i.e., they should not depend on database ids).
- References should flexibly accommodate human variation, such as alternate spellings.

The following are all valid refs:

- `Bereishit`
- `Job 3`
- `Mishna Berakhot 4.2`
- `Sanhedrin 4b`
- `Ex. 12:2-8`
- `Song_of_Songs.2.4-3.3`
- `Berakhot.2a.10-13`
- `Rashi on Genesis 1:2:1`
- `M. Peah 3`
- `Rambam Laws of Repentance 2:1`
- `Masekhet Shabbat 7b:12-20`
- `Pirkei_Avot_2.1`
- `Siddur Ashkenaz, Weekday, Shacharit, Preparatory Prayers, Modeh Ani`

A valid ref consists of a title string followed optionally by a sections string. A title string is any one of the known text titles or title variants. A section string lists sections of the text.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b9c1dec-ref.drawio_1.png",
        "",
        "Various Valid Sefaria Refs, composed of a Title String and a Sections String"
      ],
      "align": "center",
      "sizing": "70% ",
      "caption": "Various Valid Sefaria Refs, composed of a Title String and a Sections String"
    }
  ]
}
[/block]


## Section String Separators

Section Strings can be separated by any of the following characters:

[block:parameters]
{
  "data": {
    "h-0": "Separator",
    "h-1": "Name",
    "h-2": "Examples",
    "0-0": "`.`",
    "0-1": "Period",
    "0-2": "`Genesis 1.1`  \n`Berakhot 2a.1`",
    "1-0": "`,`",
    "1-1": "Comma",
    "1-2": "`Genesis 1,1`  \n`Berakhot 2a,1`",
    "2-0": "` `",
    "2-1": "Space",
    "2-2": "`Genesis 1 1`  \n`Berakhot 2a 1`",
    "3-0": "`:`",
    "3-1": "Colon",
    "3-2": "`Genesis 1:1`  \n`Berakhot 2a:1`"
  },
  "cols": 3,
  "rows": 4,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


A ranged text is indicated by use of a `-` hyphen character. Some examples of ranged refs:

- `Genesis 1:1-2`
- `Genesis 1:1-2:1`
- `Rashi on Genesis 1:1:3-2:3:1`

## Notes

- Talmud references use a special _daf_ syntax, which allows a number plus either 'a' or 'b' (for example: `Shabbat 31a` or `Pesachim 22b`). 
- Commentary text names inherit the structure (depth and section names) of their base text and add an additional level called 'Comment'. Thus `Ramban on Genesis 4:5:2` refers to Ramban's second comment on the fifth verse of Genesis Chapter 4.
- Within a sections string, `.` `,` `:`   are all interchangeable.
- The range separator ( "-" ) should appear 0 or 1 times and should never appear in the title string.
- Spaces may be replaced with \_ for better display in URLs, but not by "-" (because a dash represents a range separator).

## Classes of References Not Yet Supported

- Reference where the location of the reference indicates the target of the reference, such as `שם` or `ibid`. (Note that partial references can include `להלן`, `לקמן`,`בסימן`, etc.). 
- References by Dibbur HaMatchil
- References by Perek Name (i.e. `Keitzad Mevarchim`)
- References in the discursive flow of the text