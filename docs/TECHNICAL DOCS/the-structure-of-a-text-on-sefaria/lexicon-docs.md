---
title: Lexicon
excerpt: Dictionaries at Sefaria
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
A Lexicon (dictionary) in Sefaria is composed of a `Lexicon` object representing the whole work, and `LexiconEntry` objects for each entry.  

Additionally, a Lexicon can  have `WordForm` objects for each written expression of a word in the lexicon. `WordForm`objects represent various conjugations of words that appear in our texts, and attempt to link them to that specific headword. 

If the Lexicon is to be viewed as an independent text, it needs to have an `Index` record as well. This `Index` will have special fields, and can optionally have a `Version` record for any additional textual content which is not a dictionary entry (e.g. for introductory text to the lexicon.) 

To see documentation on our lexicon APIs, see [here](https://developers.sefaria.org/reference/get_api-lexicon)

# Understanding the Lexicon Object Model

## Lexicon

If the Lexicon has an associated `Index`, `Lexicon.index_title` must match the `title` of the `Index` object, and on the Index, `lexiconName` must match the `name` of the `Lexicon` object.  If it has a `Version` associated with it, the title of the version should be placed in the `version_title`attribute, and language of the version in the`version_lang` attribute of the Lexicon.

* `name` is the key field for the Lexicon
* `attribution`, `source` and `source_url` are descriptive.  
* `language`  
* `to_language` 
* `text_categories`

Example of a `Lexicon` object in our database: 

```
{ 
    "text_categories" : [], 
		"attribution" : "Rabbi Marcus Jastrow",
    "name" : "Jastrow Dictionary",
    "language" : "heb.talmudic",
    "version_lang" : "en",
    "to_language" : "eng",
    "index_title" : "Jastrow",
    "source" : "Jastrow Dictionary",
    "version_title" : "London, Luzac, 1903",
    "should_autocomplete" : true
}
```

## Lexicon Entry

Recall that every `Lexicon` object is comprised of `LexiconEntry` objects which represent the words in that `Lexicon`. 

Notes:

* `parent_lexicon` must match `Lexicon.name`
* `headword` - together with `parent_lexicon`, `headword `is the key for the Lexicon entry.  Must be unique for this Lexicon. 
* `prev_hw` - The headword for the entry just before this one.  (required when Lexicon is presented as a text, with an `Index`.)  
* `next_hw` - The headword for the entry just after this one.  (required when Lexicon is presented as a text, with an `Index`)  
* `rid` - unique ID.  Used for lexical sorting.  When presented in order, the rid should be in order.  `rid` values should begin with a letter, to ensure lexical and not numeric sorting. 

```
{ 
    "headword" : "אִיזְמֵיל", 
    "parent_lexicon" : "Jastrow Dictionary", 
    "rid" : "A01200", 
    "language_code" : " h. a. ch.", 
    "refs" : [
        "Chullin 31a:11", 
        "Shemot Rabbah 26"
    ], 
    "content" : {
        "senses" : [
            {
                "definition" : " (זמל √<span dir=\"rtl\">מל</span>; cmp. b. h. <span dir=\"rtl\">סמל</span>; cmp. <a dir=\"rtl\" class=\"refLink\" href=\"/Jastrow,_*אִיזְגָּרָא.1\" data-ref=\"Jastrow, *אִיזְגָּרָא 1\">אִיזְגָּרָא</a>) <i>cutting tool, knife</i>, esp. <i>surgeon’s knife</i>. <a class=\"refLink\" href=\"/Aramaic_Targum_to_Job.16.9\" data-ref=\"Aramaic Targum to Job 16:9\">Targ. Job XVI, 9</a>; a. e.—<a class=\"refLink\" href=\"/Chullin.31a.11\" data-ref=\"Chullin 31a:11\">Ḥull. 31ᵃ</a> <span dir=\"rtl\">א׳ שיש לו קרנים</span> a knife which has hornlike projections as ornaments. Y. Sabb. XIX, beg. 16ᵈ <span dir=\"rtl\">אנשון מייתי או׳</span> they had forgotten to bring the knife (for circumcision). <a class=\"refLink\" href=\"/Shemot_Rabbah.26\" data-ref=\"Shemot Rabbah 26\">Ex. R. s. 26</a> man <span dir=\"rtl\">מכה באי׳ וכ׳</span> wounds with a knife (operating) and heals &c. Pl. Chald. <span dir=\"rtl\">אִזְמֵלַיָּיא</span>; <span dir=\"rtl\">אִזְמַלְוָון</span> (f.). <a class=\"refLink\" href=\"/Targum_Jonathan_on_Isaiah.44.13\" data-ref=\"Targum Jonathan on Isaiah 44:13\">Targ. Is. XLIV, 13</a>. <a class=\"refLink\" href=\"/Targum_Jonathan_on_Joshua.5.2\" data-ref=\"Targum Jonathan on Joshua 5:2\">Targ. Josh. V, 2</a>."
            }
        ], 
        "morphology" : "m."
    }, 
    "plural_form" : [
        "אִזְמֵלַיָּיא", 
        "אִזְמַלְוָון"
    ], 
    "alt_headwords" : [
        "אִיזְמֵל", 
        "אִזְ׳", 
        "(אוּזְמֵל)"
    ], 
    "quotes" : [

    ], 
    "prev_hw" : "אִיזְמָא", 
    "next_hw" : "איזמר"
}

```

When a `Lexicon` is to be presented as a text in itself (with an `Index` record), the `LexiconEntry` objects are arranged with pointers to the entries before and after.

<br />

## WordForm

There can be many `WordForm` objects corresponding to a `LexiconEntry` and many `LexiconEntry` objects for a `WordForm`. 

Example:

```
	{ 
    "form" : "פתיח", 
    "lookups" : [
        {
            "parent_lexicon" : "Klein Dictionary", 
            "headword" : "פָּתֽיחַ ᴵ"
        }
    ], 
    "c_form" : "פתיח", 
    "refs" : [
        "Eruvin 24b:4", 
        "Megillah 26b:13"
    ], 
    "generated_by" : "prefix_adder_1"
}
```

Note: `lookups` represents the list of `LexiconEntry` objects.  There can be many objects in the list.

### Refs in WordForm

The `refs` list is meant to be a way to further restrict the correspondence between the naturally occurring word in a given text and a `LexiconEntry`. While the word may appear in the same form throughout the Sefaria library, it may have different meanings based on the context or nature of the work within which it appears. The `refs` list will associate a given definition for a word with the instances in which it appears that word has identical meanings. 

For example, if a word in Biblical Hebrew has a different meaning than when it appears in Modern Hebrew works, there will be separate `WordForm` objects: one representing the Biblical Hebrew word and associating it with `refs` from the Bible, and another representing the Modern Hebrew word and associating it with `refs` corresponding to its appearance in Modern Hebrew works. 

### Many-to-Many

When querying for a `WordForm` for a given string, one may receive many results to enable maximum flexibility in representing natural language. 

## Index

When a dictionary is presented as a text, it has a special `Index` record with a`lexiconName` element at the root, and a `DictionaryNode` element in the schema.   The `lexiconName` must match the `name` of the `Lexicon` object.  (In the reverse direction, `Lexicon.index_title` must match the `title` of the `Index` object.  

### DictionaryNode

A `DictionaryNode` can be placed anywhere within a complex schema tree.  

* `nodeType` - will be `DictionaryNode`
* `lexiconName`
* `default` - If it's true, entries can be referenced just with the dictionary name.
* `lastWord` 
* `firstWord`
* `headwordMap`

Below is the full record for the Jastrow dictionary.  Note the `lexiconName` and `DictionaryNode` element in the schema.  

```
{ 
    "categories" : [
        "Reference"
    ], 
    "schema" : {
        "key" : "Jastrow", 
        "nodes" : [
            {
                "lexiconName" : "Jastrow Dictionary", 
                "default" : true, 
                "lastWord" : "תתנא", 
                "firstWord" : "א", 
                "nodeType" : "DictionaryNode", 
                "headwordMap" : [
                    [
                        "א", 
                        "Jastrow, א"
                    ], 
                    [
                        "ב", 
                        "Jastrow, ב"
                    ], 
                    [
                        "ג", 
                        "Jastrow, ג"
                    ], 
                    [
                        "ד", 
                        "Jastrow, ד"
                    ], 
                    [
                        "ה", 
                        "Jastrow, ה"
                    ], 
                    [
                        "ו", 
                        "Jastrow, ו"
                    ], 
                    [
                        "ז", 
                        "Jastrow, ז"
                    ], 
                    [
                        "ח", 
                        "Jastrow, ח"
                    ], 
                    [
                        "ט", 
                        "Jastrow, ט"
                    ], 
                    [
                        "י", 
                        "Jastrow, י"
                    ], 
                    [
                        "כ", 
                        "Jastrow, כ"
                    ], 
                    [
                        "ל", 
                        "Jastrow, ל"
                    ], 
                    [
                        "מ", 
                        "Jastrow, מ"
                    ], 
                    [
                        "נ", 
                        "Jastrow, נ"
                    ], 
                    [
                        "ס", 
                        "Jastrow, ס"
                    ], 
                    [
                        "ע", 
                        "Jastrow, ע"
                    ], 
                    [
                        "פ", 
                        "Jastrow, פ"
                    ], 
                    [
                        "צ", 
                        "Jastrow, צ"
                    ], 
                    [
                        "ק", 
                        "Jastrow, ק"
                    ], 
                    [
                        "ר", 
                        "Jastrow, ר"
                    ], 
                    [
                        "ש", 
                        "Jastrow, שׁ"
                    ], 
                    [
                        "ת", 
                        "Jastrow, ת"
                    ]
                ]
            }, 
            {
                "key" : "Preface", 
                "addressTypes" : [
                    "Integer"
                ], 
                "sectionNames" : [
                    "Paragraph"
                ], 
                "nodeType" : "JaggedArrayNode", 
                "depth" : 1, 
                "titles" : [
                    {
                        "primary" : true, 
                        "lang" : "he", 
                        "text" : "הקדמה"
                    }, 
                    {
                        "primary" : true, 
                        "lang" : "en", 
                        "text" : "Preface"
                    }
                ]
            }, 
            {
                "key" : "Hebrew or Aramaic Abbreviations", 
                "addressTypes" : [
                    "Integer"
                ], 
                "sectionNames" : [
                    "Line"
                ], 
                "nodeType" : "JaggedArrayNode", 
                "depth" : 1, 
                "titles" : [
                    {
                        "primary" : true, 
                        "lang" : "he", 
                        "text" : "קיצורים בעברית או בארמית"
                    }, 
                    {
                        "primary" : true, 
                        "lang" : "en", 
                        "text" : "Hebrew or Aramaic Abbreviations"
                    }
                ]
            }, 
            {
                "key" : "List of Abbreviations", 
                "addressTypes" : [
                    "Integer"
                ], 
                "sectionNames" : [
                    "Line"
                ], 
                "nodeType" : "JaggedArrayNode", 
                "depth" : 1, 
                "titles" : [
                    {
                        "primary" : true, 
                        "lang" : "he", 
                        "text" : "רשימת קיצורים"
                    }, 
                    {
                        "primary" : true, 
                        "lang" : "en", 
                        "text" : "List of Abbreviations"
                    }
                ]
            }
        ], 
        "titles" : [
            {
                "primary" : true, 
                "lang" : "he", 
                "text" : "מילון יסטרוב"
            }, 
            {
                "primary" : true, 
                "lang" : "en", 
                "text" : "Jastrow"
            }
        ]
    }, 
    "enDesc" : "A Dictionary of the Targumim, the Talmud Bavli and Yerushalmi, and the Midrashic Literature", 
    "order" : [

    ], 
    "is_cited" : false, 
    "pubPlace" : "New York", 
    "compPlace" : "Philadelphia", 
    "lexiconName" : "Jastrow Dictionary", 
    "pubDate" : "1903", 
    "title" : "Jastrow", 
    "era" : "CO", 
    "errorMargin" : "10", 
    "authors" : [
        "Marcus Jastrow"
    ], 
    "compDate" : "1893"
}

```

## Version

Necessary for any regular (non-definition) text.

## Important Notes

* In `sefaria/model/lexicon.py`, each dictionary relates to a subclass of `DictionaryEntry`.  Those correspondences are listed in `LexiconEntrySubClassMapping`
* In `sefaria.js`, there is a line that lists the dictionaries:  `Sefaria.virtualBooksDict = [...]`
* If this lexicon participates in the cross-dictionary auto-completer, it needs to be listed in `library.build_lexicon_auto_completers`
