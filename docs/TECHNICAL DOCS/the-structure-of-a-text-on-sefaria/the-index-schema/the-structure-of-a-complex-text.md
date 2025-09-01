---
title: The Structure of a Complex Text
excerpt: >-
  Exploring the Index schema of "complex" texts and how they work in structuring
  texts at Sefaria
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
      slug: node-titles
      title: Node Titles
---
# Complex Schemas (Multi-Node Trees)

Texts which are more complex than a simple `Jagged Array` need a complex Index schema.  Complex schemas are always structured as trees made up of many nodes, usually a combination of type `SchemaNode` and `JaggedArrayNode`.  Each node has a `key` and `titles`. If it is a `SchemaNode`, it will have other nodes as children; otherwise it is a `JaggedArrayNode` describing `JaggedArray` content.  

_Note: The `titles` blocks in all of the examples below were left as empty arrays for the sake of brevity, and `sharedTitle` were omitted for the same reasons. We will dive deeper on titles in [Schema Node Titles](doc:node-titles)_

### 

## Example 1: A "Simple" Complex Text

Let's look at a moderately complex text, and then at the Index Schema that describes it. This text has the following three sections:

1. **Introduction** - Structured as a series of paragraphs
2. **Main Body** - Structured by chapters containing sections
3. **Conclusion** - Structured as a series of paragraphs

The text looks like this when viewed in a `JaggedArray`:

```python
{
    "Introduction": ["Intro Paragraph 1", "Intro Paragraph 2", ...],
    "Contents": [
      						["Chapter 1, Section 1", "Chapter 1, Section 2"],
                  ["Chapter 2, Section 1", "Chapter 2, Section 2"], 
                 ...],
    "Conclusion": ["Conclusion Paragraph 1", "Conclusion Paragraph 2", ...]
}

```

The schema, as serialized in the `Index` record, looks something like this:

```json
 "schema" : {
        "nodes" : [
            {
                "nodeType" : "JaggedArrayNode",
                "depth" : NumberInt(1),
                "addressTypes" : ["Integer"],
                "sectionNames" : ["Paragraph"],
                "titles": [...],
                "key" : "Introduction"
            },
            {
                "nodeType" : "JaggedArrayNode",
                "depth" : NumberInt(3),
                "addressTypes" : ["Perek","Pasuk","Integer"],
                "sectionNames" : ["Chapter","Verse","Comment"],
                "default" : true,
                "key" : "default"
            },
            {
                "nodeType" : "JaggedArrayNode",
                "depth" : NumberInt(1),
                "addressTypes" : ["Integer"],
                "sectionNames" : ["Paragraph"],
                "titles": [...],
                "key" : "Conclusion"
            }
        ],
        "nodeType" : "SchemaNode",
        "titles" : [...],
        "key" : "Example Text"
    }
```

The root node in the schema, besides the required `key` and `titles` attributes, has an attribute called `nodes`.  `nodes `is a list of dictionaries, with each of those dictionaries itself containing a node.  Look at the keys of each of those children.  They match the keys of the dictionary of the text.  Each of the child nodes in the Index record describes how that section of the text is structured.  

Let's take a step back, and conceptualize the structure of this Index record as a tree:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ef788d0-complex_schema.drawio.png",
        "",
        ""
      ],
      "align": "center",
      "sizing": "75% "
    }
  ]
}
[/block]


You'll see that at the root of the tree there is a `SchemaNode` representing the entirety of the text. This `SchemaNode` has three `JaggedArrayNode` children, each representing the content of that section which is stored on the `Version` in an associated `JaggedArray`. 

## Example 2: Abarbanel on Torah

For a more complicated example of a complex text, let's look at the [Abarbanel on Torah](https://www.sefaria.org/Abarbanel_on_Torah?tab=contents). The Abarbanel on Torah has a more nuanced structure, with multiple layers of `SchemaNode`nodes. The Abarbanel has a verse-by-verse commentary on each of the five books of Torah, and for each book he also writes an introduction to his commentary on that book. 

The schema as it appears on the Index record can be seen in the snippet below.

### The Index Record Schema for Abarbanel on Torah

```json
 "schema" : {
        "nodes" : [
            {
                "nodes" : [
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(1),
                        "addressTypes" : ["Integer"],
                        "sectionNames" : ["Paragraph"],
                        "key" : "Introduction"
                    },
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(3),
                        "addressTypes" : ["Perek","Integer","Integer"],
                        "sectionNames" : ["Chapter","Verse","Paragraph"],
                        "default" : true,
                        "key" : "default"
                    }
                ],
                "titles" : [...],
                "key" : "Genesis"
            },
            {
                "nodes" : [
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(1),
                        "addressTypes" : ["Integer"],
                        "sectionNames" : ["Paragraph"],
                        "key" : "Introduction"
                    },
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(3),
                        "addressTypes" : ["Perek","Integer","Integer"],
                        "sectionNames" : ["Chapter","Verse","Paragraph"],
                        "default" : true,
                        "key" : "default"
                    }
                ],
                "titles" : [...],
                "key" : "Exodus"
            },
            {
                "nodes" : [
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(1),
                        "addressTypes" : ["Integer"],
                        "sectionNames" : ["Paragraph"],
                        "key" : "Introduction"
                    },
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(3),
                        "addressTypes" : [
                            "Perek",
                            "Integer",
                            "Integer"
                        ],
                        "sectionNames" : [
                            "Chapter",
                            "Verse",
                            "Paragraph"
                        ],
                        "default" : true,
                        "key" : "default"
                    }
                ],
                "titles" : [...],
                "key" : "Leviticus"
            },
            {
                "nodes" : [
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(1),
                        "addressTypes" : ["Integer"],
                        "sectionNames" : ["Paragraph"],
                        "key" : "Introduction"
                    },
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(3),
                        "addressTypes" : ["Perek","Integer","Integer"],
                        "sectionNames" : ["Chapter","Verse","Paragraph"],
                        "default" : true,
                        "key" : "default"
                    }
                ],
                "titles" : [...],
                "key" : "Numbers"
            },
            {
                "nodes" : [
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(1),
                        "addressTypes" : ["Integer"],
                        "sectionNames" : ["Paragraph"],
                        "key" : "Introduction"
                    },
                    {
                        "nodeType" : "JaggedArrayNode",
                        "depth" : NumberInt(3),
                        "addressTypes" : ["Perek","Integer","Integer"],
                        "sectionNames" : ["Chapter","Verse","Paragraph"],
                        "default" : true,
                        "key" : "default"
                    }
                ],
                "titles" : [...],
                "key" : "Deuteronomy"
            }
        ]
```

You'll see that the `schema` is a JSON object, with a `nodes` key that contains an array of five JSON objects, each one representing a `SchemaNode` pointing to the introduction and commentary corresponding to each book of Torah. Each of these JSON objects has a `nodes` key, which points to an array of the two `JaggedArrayNodes` which correspond to the `JaggedArray` for the introduction and the commentary for the given book. 

Let's visualize this as a tree:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/79a8c38-abarbanel_complex.drawio.png",
        null,
        "The Schema of Abarbanel on Torah as a Tree"
      ],
      "align": "center",
      "sizing": "125% ",
      "caption": "The Schema of Abarbanel on Torah as a Tree"
    }
  ]
}
[/block]


You can see in the above diagram that the Abarbanel on Torah has two layers of `SchemaNode` nodes (represented by the purple circles). The root is the `SchemaNode` representing the entire Index. The immediate children are each a `SchemaNode` representing the Abarbanel's commentary on one of the Five Books of Torah. The children of each "book-level" `SchemaNode` are `JaggedArrayNode`nodes (represented by the yellow rectangles). Each book has a `JaggedArrayNode` for the Introduction (with a key of `Introduction`) and a `JaggedArrayNode` for the main body of the commentary on that book (with a key of `default`, more on that in [Default Nodes](doc:default-nodes)). Each `JaggedArrayNode` corresponds to an actual `JaggedArray` (i.e. a list of lists, represented by the loosely associated blue rectangles) on the `Version` of the text containing the actual text of that corresponding section of the Index. 

# Summary

Schema trees can descend to any depth.  Each Index will have different structures, varying greatly in composition and complexity. It is crucial to understand `JaggedArray`, `JaggedArrayNode`, and `SchemaNode`. With these building blocks, one can understand how `Index` schema trees are built, and how to read the schema as presented in serialized form on the `Index` record. This knowledge underlies every work in the Sefaria library, and is key to a deep understanding of how we store and structure text.