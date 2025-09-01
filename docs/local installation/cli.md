---
title: CLI
excerpt: The Sefaria Command-Line-Interface tool.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Sefaria's Command Line Interface (CLI) enables users to interact directly with Sefaria's models and internal functions, providing a powerful and efficient way to access and utilize the platform's capabilities locally. By leveraging the CLI, users can bypass the need for API calls, reducing latency and improving performance.

One of the key advantages of using Sefaria's CLI is the ability to access and manipulate data offline. This feature is particularly useful for researchers, scholars, and developers who require quick and reliable access to Sefaria's vast library of Jewish texts and resources, even in situations where internet connectivity is limited or unavailable. 

> 🚧 Local Install Required
>
> Please note, use of the Sefaria CLI requires a full [local installation](https://dash.readme.com/project/sefaria/v1.0/docs/local-installation-instructions) of the project.

## Understanding the Shell Script

The CLI itself doesn't have any of its own functionality. All of the CLI functionality is inherited from the Python data models. The entirety of the code in `cli.py` is:

```python python
import django
django.setup()

from sefaria.model import *
import sefaria.system.database as database

```

The shell script can be accessed by running `./cli` from the root of the project. It sets the appropriate environment variables, and starts up a Python interpreter with `sefaria.model`(using the Django context) loaded into the global namespace.  For users of iPython, you can load open a session in iPython with `./cli -i`.

## Seeing all Object Properties and Functions

For users of iPython, you can load open a session in iPython with `./cli -i`.  You can instantiate an object, then reference the assigned variable, followed by a `.`, press tab and see the properties and functions of a given object. 

 For example, here are the properties and functions available on an instance of the `Ref` object. 

<Image align="center" src="https://files.readme.io/f8f0537f20f64330106a77101182da0b9862f0d662a3bc3299f2e6038520c664-Screenshot_2025-01-23_at_14.15.22.png" />

Using 'tab' can be very helpful for getting a birds-eye view of the many existing properties and functions available on Sefaria objects. 

## CLI Examples

Before running any of the following examples, make sure you've entered the Sefaria CLI by running `./cli` from the root of the project directory. 

### Link Counts

The example below counts the links to Genesis 13. (*Note: Your results may vary, as more links have likely been added since we generated this code*). 

```python
$ ./cli
>>> links_for_genesis_ref = LinkSet(Ref("Genesis 13"))
>>> links_for_genesis_ref.count()
226

```

### Text Segments

The example below retrieves the first verse of the book of Genesis. 

```python
>>> book = library.get_index("Genesis")
>>> first_ref = book.all_segment_refs()[0]
>>> tc = first_ref.text()
>>> tc.as_string()
'When God began to create<sup class="footnote-marker">*</sup><i class="footnote"><b>When God began to create </b>Others “In the beginning God created.”</i> heaven and earth—'
```

### Versions

This example retrieves a list containing the available versions for the book of Genesis. (For the sake of brevity, we truncated the returned list to the first three). 

```python
>>> book = library.get_index("Genesis")
>>> [v.versionTitle for v in book.versionSet()]
['The Contemporary Torah, Jewish Publication Society, 2006',
 'The Five Books of Moses, by Everett Fox. New York, Schocken Books, 1995',
 'The Koren Jerusalem Bible',
...]

```

If you have a local installation set up, feel free to play around and see what you can discover via the Sefaria CLI tool.
