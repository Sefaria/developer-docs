---
title: The Index Schema
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    The document explains the structure of book index schemas in Sefaria, which
    are tree-like structures composed of SchemaNode and JaggedArrayNode objects.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: jaggedarray-and-jaggedarray-nodes
      title: JaggedArray and JaggedArray Nodes
---
# Index Schemas

The structure of a book is defined in the `Index` schema.  These schemas are [trees](https://en.wikipedia.org/wiki/Tree_(data_structure)) made up of nodes.  
In the Python [code](https://github.com/Sefaria/Sefaria-Project/blob/fe9c971217b99cd763c0dd8f42b82339379b961c/sefaria/model/schema.py#L1218), these nodes are instances of the class `sefaria.model.schema.SchemaNode` and its children.  The trees are stored in the database and transmitted through the API in a serialized form. 

# Node Types for an `Index` schema tree

Index schemas are structured as trees, where in most cases the internal nodes of the tree are of type `SchemaNode`  and the leaves of the tree are of type`JaggedArrayNode`(which will be explained in the [next section](doc:jaggedarray-and-jaggedarray-nodes)). 

Below is a diagram of the class hierarchy that defines the various Index schema nodes. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b4a570b-index_schema_hierarchy.png",
        "",
        "Inheritance Hierarchy of Index Schema Nodes at Sefaria"
      ],
      "align": "center",
      "sizing": "75% ",
      "caption": "Inheritance Hierarchy of Index Schema Nodes at Sefaria"
    }
  ]
}
[/block]


Since not all of this information is immediately relevant to the majority of engineers engaging in projects with Sefaria data, we only elaborate on `SchemaNode` and `JaggedArrayNode` in these docs. For a deeper dive, you can explore more of the schema on [GitHub](https://github.com/Sefaria/Sefaria-Project/blob/master/sefaria/model/schema.py).