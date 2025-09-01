---
title: Shape API
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: categories
      title: Categories
---
# Shape API

The shape API allows one to retrieve information about the shape of an `Index` on Sefaria. The shape refers to some basic statistics about the `Index`- most prominently the number of chapters and segments per chapter. 

## GET /api/shape/:title

### Path Parameters

`title` - A valid node title or a path to a category, separated by `/`.

### Example requests:

- `/api/shape/Genesis`
- `/api/shape/Pesach_Haggadah`
- `/api/shape/Talmud/Bavli`

### Query Parameters

The following two query parameters can be passed in calls to the Shape API to further filter results. 

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Default value",
    "h-3": "Explanation",
    "0-0": "`depth`",
    "0-1": "`int`",
    "0-2": "`2`",
    "0-3": "The `depth` parameter in the query string indicates how many levels in the category tree to descend.  \nIf `depth=0`is passed, then the returned JSON descends to end of tree.",
    "1-0": "`dependents`",
    "1-1": "`bool`",
    "1-2": "`false`",
    "1-3": "The `dependents` parameter, if true, includes dependent texts.  By default, they are filtered out."
  },
  "cols": 4,
  "rows": 2,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


### Example Responses

#### Simple Texts

For simple texts, the response returns a dict with keys that contains the following fields:

```
{
	"section": Category immediately above book
	"heTitle": Hebrew title of node
	"length": Number of chapters
	"chapters": List of Chapter Lengths 
	"title": English title of node
	"book": English title of Book
}
```

Sample response for `sefaria.org/api/shape/Jonah`:

```
[
  {
    "section": "Prophets",
    "heTitle": "יונה",
    "title": "Jonah",
    "length": 4,
    "chapters": [
      16,
      11,
      10,
      11
    ],
  "book": "Jonah",
  "heBook": "יונה"
  }
]
```

#### Complex Texts

For complex texts or categories, the Shape API will return a list of dicts for each text within that category or text. 

Here is a snippet of what is returned when querying the Shape API for the category `Liturgy`. To see the full response, [try the full query here](https://www.sefaria.org/api/shape/Liturgy). 

```
[
  ...
	{
		"section": "Siddur",
		"heTitle": "סידור אשכנז, ימי חול, תפילת שחרית, הכנה לתפילה, מה טובו",
		"title": "Siddur Ashkenaz, Weekday, Shacharit, Preparatory Prayers, Ma Tovu",
		"length": 1,
		"chapters": 1,
		"book": "Siddur Ashkenaz",
		"heBook": "סידור אשכנז"
	},
	{
		"section": "Siddur",
		"heTitle": "סידור אשכנז, ימי חול, תפילת שחרית, הכנה לתפילה, אדון עולם",
		"title": "Siddur Ashkenaz, Weekday, Shacharit, Preparatory Prayers, Adon Olam",
		"length": 1,
		"chapters": 10,
		"book": "Siddur Ashkenaz",
		"heBook": "סידור אשכנז"
	},
	{
		"section": "Siddur",
		"heTitle": "סידור אשכנז, ימי חול, תפילת שחרית, הכנה לתפילה, יגדל",
		"title": "Siddur Ashkenaz, Weekday, Shacharit, Preparatory Prayers, Yigdal",
		"length": 1,
		"chapters": 27,
		"book": "Siddur Ashkenaz",
		"heBook": "סידור אשכנז"
	}
...
]
```