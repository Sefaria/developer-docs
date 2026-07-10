---
title: Workflowy
excerpt: >-
  When working with local installation, tools like our Workflowy Parser may be
  helpful for creating indices and adding text on Sefaria.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# What is Sefaria's Workflowy Parser?

This tool is designed to integrate [Workflowy](https://workflowy.com/) capabilities so developers can create complex texts in Sefaria with ease. Sefaria's content team has used Workflowy to create `Index` objects for over 300 books in the Sefaria Library, including resources such as [Mesilat Yesharim](https://www.sefaria.org/Mesilat_Yesharim?tab=contents), [Horeb](https://www.sefaria.org/Horeb?tab=contents), [Shulchan Arukh HaRav](https://www.sefaria.org/Shulchan_Arukh_HaRav?tab=contents), [Guide for the Perplexed](https://www.sefaria.org/Guide_for_the_Perplexed?tab=contents), and [From Sinai to Ethiopia,](https://www.sefaria.org/From_Sinai_to_Ethiopia?tab=contents) among others. This is an important tooling option for `Index` creation that usually doesn't require any engineering skills.

## What is Workflowy?

[Workflowy](https://workflowy.com/) is a versatile web-based tool for organizing information through nested lists, offering features such as tagging, collaboration, and offline access. [Workflowy](https://workflowy.com/) allows users to build nested lists that can be exported to an `.opml` file. Once exported, the lists can be parsed and ingested as an `Index` or `Version` in the Sefaria Library.

## How Can We Use Workflowy To Add Texts to Sefaria?

As mentioned above, we can use [Workflowy](https://workflowy.com/) to create a table of contents for a text (an `Index` record), and to even fill in simple text for that Index (a `Version`).

This involves two main steps:

1. Creating a [Workflowy](https://workflowy.com/) document representing the text
2. Uploading the `.opml` file on your local instance of Sefaria

# Step 1: Preparing the [Workflowy](https://workflowy.com/) Document:

[Workflowy](https://workflowy.com/) stores nodes in outline form. The parsing script mentioned above will utilize this form to create a layered table of contents.

First create a new [Workflowy](https://workflowy.com/) Document.

1. If you don't already have an account, you'll need to sign up for one (it's free).
2. Then, use their online interface to create a new document

## Defining Book Structure

Below are instructions for using Workflowy to define a book structure:

1. Add a SINGLE bullet to the main page containing the title of the book being described (All other bullets describing the internal structure of the text **MUST** be nested under this bullet).
2. Separate the English title from the Hebrew title (both are required) by using the forward slash `/` character on the keyboard.
3. If you wish to add alternate English or Hebrew titles, please group each language together (i.e. do not write one English then one Hebrew and then another in English) separated by the `/` character. Each of the languages should internally separate the alternate title variations using the pipe `|` character (usually above the enter key).
4. Proceed to add nested bullets as needed to describe the text structure. The same rules of adding titles apply to each of the nested bullets.

#### An Example:

- Siddur A / סידור א
  - Shacharit / תפילת שחרית
    - Minchah / תפילת מנחה
    - Maariv|Arvit / מעריב|ערבית
      - Vehu Rachum / והוא רחום

## Specifying Segment Depth

The deepest bullet at any point will be the one where text is actually stored on. By default, this means that you can only create a series of paragraphs at a single level of depth at this point. For example, a bullet titled "The Tale of the Four Kings", unless otherwise specified, will only be able to have references such as `The Tale of the Four Kings.1`, `The Tale of the Four Kings.2` etc.

This means that if you want a certain bullet title to also use numeric continuation at a depth larger than one (such as Chapter and Verse), you must specify this in square brackets `[]` after the titles.

#### Examples

##### Using only a number to denote depth

- Midrash on Kings
  - Introduction
  - The Tale of the Four Kings \[2]

##### Using section names (and the depth is implied from the number of section names):

- Midrash on Kings
  - Introduction
  - The Tale of the Four Kings \['Chapter', 'Verse']

##### Using both section names and types:

- Midrash on Kings
  - Introduction
  - The Tale of the Four Kings \["Chapter:Integer", "Verse:Integer"]

## Default Titles

Sometimes you will find yourself with a structure like this:

- The Tale of the Four Kings
  - Introduction
  - The Tale of the Four Kings

In such a case, we use a notion called a default node in order to eliminate the title repetition. To accomplish this, simply replace the redundant title with the special string `\*\*default\*\*`:

#### Example of an Index Outline with a Default String

- The Tale of the Four Kings
  - Introduction
  - `\*\*default\*\*`

## Specifying Categories

If you wish to specify categories in the Sefaria table of contents (e.g. Talmud->Bavli->Seder Zeraim), please add them to the root bullet, after the titles. Surround this categories text with the percent sign `%`. The categories themselves should be separated from each other by a comma (e.g. Talmud,Bavli,Seder Zeraim).

#### Example of a Text with Categories

- Modern Commentary on Esther / פירוש מודרני על מגילת אסתר %Tanakh,Commentary,Modern Commentary%
  - Introduction / הקדמה
    - Part One / חלק א׳
    - Part Two / חלק ב׳
  - `\*\*default\*\*`

## Important Notes

### Delimiters and Forbidden Characters

The following characters are used as delimiters, and therefore may **NOT** be used inside of any title:

- `/` - The forward slash
- `|` - The pipe character

Additionally, do **not** use a hyphen (i.e. `-`) inside titles. The titles are used to craft URLs to Sefaria and a hyphen is an illegal character inside a URL.

### Comments

Should you need to make a comment that will not be parsed, please place the text surrounded by the pound sign `#` parentheses. It will then be ignored.

#### Example

- Modern Commentary on Esther / פירוש מודרני על מגילת אסתר %Tanakh,Commentary,Modern Commentary%
  - Introduction / הקדמה
    - Part One / חלק א׳ # Remember to get the text for this!
    - Part Two / חלק ב׳ # I have the text in a .docx file, must convert!
  - `\*\*default\*\*`

## Optional Step: Adding optional text to the Workflowy ouline

In some cases, entering the version text into the Workflowy rather than later through the Sefaria GUI might be preferred. If so, text can be added to outline nodes using the Workflowy "add note" feature. Simply hover the cursor over the appropriate bullet and select "add note". Please note that only text of depth 1 can be added, but not of any higher depth (.e.g. a list of verses can be added, but a list of chapters with verses inside them cannot)

### Parsing Text Formatting

A few standards are used by this script when parsing:

- Text inside parenthesis `()` - is italicized (`<em>`).
- A paragraph break \[ie: the enter key] separates paragraphs.
- Forward slashes `/` are interpreted as line breaks.
- iI you need to list version attributes (i.e. versionTitle, versionSource, etc) use the notes on the primary text title (the topmost title- for the whole book) as that will usually not have other text under it.

## Exporting the Workflowy:

To export, simply hover the cursor over the bullet of a root node in the Workflowy. Select "export" and choose opml. Save to `.opml` file.

### Example `.opml` File

A sample `.opml` file can be seen below:

```html OPML
<?xml version="1.0"?>
<opml version="2.0">
  <head>
    <ownerEmail>
      sefaria-fan@my-email.com
    </ownerEmail>
  </head>
  <body>
    <outline text="Text" _note="Version is ABCDE&#10;">
      <outline text="My Text | Textual Text / הטקסט שלי %Tanakh,Modern Commentary%">
        <outline text="Chapter 1 / פרק א׳" _note="The first verse,The second verse,The third verse&#10;" />
        <outline text="Chapter 2 / פרק ב׳" />
      </outline>
    </outline>
  </body>
</opml>
```

# Step 2: Uploading the Index via Moderator Tools

To upload the text to Sefaria, make sure your local installation of the project is running.

Log-in to your local user account, make sure you have admin permissions set.

Navigate to `/modtools`, on most machines this will be at [http://127.0.0.1:8000/modtools](http://127.0.0.1:8000/modtools). Scroll down to the `Workflowy Outline Upload` section, pictured below:


<Image src="https://files.readme.io/a93b28b-Screen_Shot_2024-02-21_at_12.58.33.png" align="center" width="60% " border={true} />


Hit the button `choose file`, and select the `.opml` file downloaded from Workflowy. Select whether or not you are just creating an `Index` or also adding a `Version` of text. When complete, hit `upload` to upload your text to your local Sefaria database.

Upon success, in the text box beneath the `upload` button you will see the full `Index` record for the newly created text:


<Image src="https://files.readme.io/e067bb5-Screen_Shot_2024-02-21_at_12.47.22.png" align="center" width="60% " border={true} />


(To see this in the code, navigate to [Sefaria-Project/sefaria/views.py](https://github.com/Sefaria/Sefaria-Project/blob/54f78cb72a2c071261cee3cfd8317141fc5bed9d/sefaria/views.py#L1334) to see the `modtools_upload_workflowy()` function).
