---
title: JaggedArray and JaggedArray Nodes
excerpt: >-
  JaggedArray and JaggedArrayNodes are objects used for representing the text
  content, and the structure of simple texts.
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    The document explains the structure and use of JaggedArrayNodes and
    JaggedArrays in organizing text data, particularly in representing texts
    like the Tanakh, where chapters are sections and verses are segments within
    a nested list structure.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: schemanode
      title: SchemaNode
---
# Content Nodes

Content nodes describe the structure that the text is stored in, from that level of the tree on.  Currently, all content nodes are `JaggedArrayNodes`, nodes which describe a `JaggedArray` (list of lists) structure.

# `JaggedArray`

A `JaggedArray` is a nested array (i.e. a list of lists) of a certain depth, with the lowest level being an array of strings.  A `JaggedArray` with a depth of 2 is an array of arrays of strings. A common example of a text represented by a `JaggedArray` of a depth of 2 is the structure of a book of Tanakh. A book of Tanakh has many chapters, and each chapter has many verses.  Each chapter is represented by an array within the outer array, and contains numerous strings, with each string representing a verse. The position of an item in the array reflects the structural information about that item.  

### An Example of a `JaggedArray` of `depth=2`

The example below mocks a `JaggedArray` for a text with four chapters, each of those chapters containing a variable number of verses. Each sub-array represents a chapter. Each string inside the sub-array is a verse of the given chapter. 

```python
[
  ["Text of 1:1", "Text of 1:2", ], # Chapter 1
  ["Text of 2:1", "Text of 2:2", "Text of 2:3", "Text of 2:4", "Text of 2:5"], # Chapter 2
  ["Text of 3:1", "Text of 3:2", "Text of 3:3"], # Chapter 3
  ["Text of 4:1", "Text of 4:2", "Text of 4:3", "Text of 4:4"] # Chapter 4

]
```

We refer to these objects as a `JaggedArray` since any spot in the array could be empty, as many of our versions of text are incomplete. 

# `JaggedArrayNode`

A `JaggedArrayNode` in the schema of an `Index` is a node which indicates that there will be a `JaggedArray` present to represent this part of the text in the `Version` record.   The simplest example is a schema that consists of a single `JaggedArrayNode`. In the `Version`, the entire book exists within a single array.

## Segments and Sections

In Sefaria, we refer to the strings at the bottom of the `JaggedArray` as **segments**. The arrays that contain those strings are called **sections**. In general, segments reflect the maximum resolution that we are capable of reaching in a given text - one cannot directly reference a part of a segment.

For example, in a book of Tanakh which is organized into chapters and verses, a chapter would be a **section** and a verse would be considered a **segment**.