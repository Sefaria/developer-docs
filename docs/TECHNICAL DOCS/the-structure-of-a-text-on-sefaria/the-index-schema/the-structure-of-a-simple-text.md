---
title: The Structure of a Simple Text
excerpt: Exploring the Index schema for "simple" texts at Sefaria.
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
      slug: the-structure-of-a-complex-text
      title: The Structure of a Complex Text
---
# The Schema of a Simple Text

The simplest Index records in the system have schema trees with just one node - a content node.  Specifically, they have a [`JaggedArrayNode`](doc:jaggedarray-and-jaggedarray-nodes), which is a type of content node.

<Image alt="A Single-Node-Tree for a Simple Text" align="center" width="50% " src="https://files.readme.io/2a6915c-complex_schema.drawio_1.png">
  A Single-Node-Tree for a Simple Text
</Image>

For each `JaggedArrayNode`, there is a corresponding `JaggedArray` on the Version containing the text  (not included in the diagram). 

## Example: The Book of Genesis

An example of a simple schema is the book of Genesis.  The text for Genesis is stored in a depth 2 `JaggedArray` - an array of arrays. Each array in the `JaggedArray `represents a chapter, and each element within the chapter is an array of strings, representing verses. 

The content for this book looks like this:

```python
[
  ["Verse 1:1", "Verse 1:2", ...] # Chapter 1
  ["Verse 2:1", "Verse 2:2", ...] # Chapter 2
  [...] 
]
```

The Index schema describing the book looks like this.  

```json
{
    "title" : "Genesis",
    "maps" : [],
    "order" : [1, 1],
    "categories" : ["Tanach", "Torah"],
    "schema" : {
        "titles" : [ 
            {
                "lang" : "en",
                "text" : "Genesis",
                "primary" : True
            }, 
            {
                "lang" : "en",
                "text" : "Bereishit"
            }, 
            {
                "lang" : "he",
                "text" : "בראשית",
                "primary" : True
            }
        ],
        "nodeType" : "JaggedArrayNode",
        "lengths" : [50, 1533],
        "depth" : 2,
        "sectionNames" : ["Chapter", "Verse"],
        "addressTypes" : ["Integer", "Integer"],
        "key" : "Genesis"
    }
}
```

Let's dive into the various properties on the schema:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Property
      </th>

      <th>
        Value in 

        `Genesis`
      </th>

      <th>
        Explanation
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `key`
      </td>

      <td>
        `Genesis`
      </td>

      <td>
        A text field.  For a single node schema like this, the value of `key` is the same as the value of the `title"`field on the Index record.
      </td>
    </tr>

    <tr>
      <td>
        `nodeType`
      </td>

      <td>
        `JaggedArrayNode`
      </td>

      <td>
        This corresponds to a related class in the Python code. The value, `JaggedArrayNode`, is currently the only one that is always used for single-node classes.
      </td>
    </tr>

    <tr>
      <td>
        `titles`
      </td>

      <td>
        ```
        [  
           {  
            "lang" : "en",  
            "text" : "Genesis",  
            "primary" : True  
            },  
            {  
             "lang" : "en",  
             "text" : "Breishit"  
             },  
             {  
             "lang" : "he",  
             "text" : "בראשית",  
             "primary" :True  
              }  
         ]
        ```
      </td>

      <td>
        An array of dictionaries specifying titles for this node (for full description of how titles work, see [Titles](doc:node-titles). Each title dictionary has two required keys:  

        * `text` : The title string
        * `lang`: Either `"en"` or `"he"`
        * `primary`: This field needs to be present and `True` for exactly one Hebrew and one English title. 
      </td>
    </tr>

    <tr>
      <td>
        `depth`
      </td>

      <td>
        `2`
      </td>

      <td>
        The depth of the `JaggedArray`. A two dimensional array (i.e. a list of lists) would have a depth of 2, and a three dimensional array (i.e. a list of lists of lists) would have a depth of 3, etc. 
      </td>
    </tr>

    <tr>
      <td>
        `addressTypes`
      </td>

      <td>
        `["Integer", "Integer"]`
      </td>

      <td>
        Array with `depth` number of values, each one indicating how that level of the `JaggedArray` is addressed.  Most commonly, these values are `Integer`, but could also be `Talmud`, or some less common values defined in `safaria.model.schema`
      </td>
    </tr>

    <tr>
      <td>
        `sectionNames`
      </td>

      <td>
        `["Chapter","Verse"]`
      </td>

      <td>
        Array with `depth` number of values, each one a string name for that level of the `JaggedArray`.
      </td>
    </tr>

    <tr>
      <td>
        `toc_zoom`
      </td>

      <td>
        n/a
      </td>

      <td>
        An  Integer value primarily used to adjust the way we choose to organize commentaries around the base text (and therefore not present on the `Index` of `Genesis`).  

        Usually, a commentary is organized by the segments of commentary on the verse of the base text it comments on. Adjusting the `toc_zoom` will allow you to display the commentary on a verse-by-verse basis (section), or a chapter-by-chapter basis (super-section), or based on a different level in the index depth.  

        `toc_zoom` sets the depth for display in the table of contents according to the following values:\
        `0` will display segments (each string in the Jagged Array).\
        `1` will display sections.\
        `2` will display super-sections.\
        If not set, the table of contents will display the section level (or segment level for depth 1 texts).  

        An example of a commentary `Index` with an adjusted `toc_zoom` can be found [here](https://www.sefaria.org/Em_LaMikra%2C_Genesis.1.1.2?lang=en\&with=all\&lang2=en), where comments are aggregated by verse of the base text for display (sections), instead of individual comments (segments). 
      </td>
    </tr>

    <tr>
      <td>
        `lengths`\
          *(optional)*
      </td>

      <td>
        `[50, 1533]`
      </td>

      <td>
        Array with up to `depth` number of values, each one an integer specifying how many element exist at that level of the `JaggedArray`. In this case, we see that `Genesis` has `50` chapters, and `1533` verses. 
      </td>
    </tr>
  </tbody>
</Table>
