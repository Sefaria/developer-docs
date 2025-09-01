---
title: How to Format Citations for the Linker
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Citation Format

We've worked pretty hard to help the linker capture as many references as possible.  In general, if the title is written out in full, followed by the numeric references, it will work well.  Every text on Sefaria can be referenced, but some of them are trickier than others. 

Going into a bit more detail:

- Punctuation shouldn't be an issue - space, comma, period, and colon in a reference will all be fine. 

- Dash has special meaning - it indicates a range (see the examples, below.)  Any extra dash will trip us up.  

- For each text, we recognize a number of common spelling variants.  For example, for "2 Kings", we'll recognize the titles: Melachim Bet, Melachim II, Second Kings, 2 Kings, II Melachim, Kings II, II Melakhim, and II Kings.  

- We'll recognize chapter level and verse level references, as well as ranges, so all of these are valid:

  - `Second Kings 2`
  - `II Kings, 2:5`
  - `II Kings 2 5`
  - `Kings II 2-4`
  - `2 Kings 2:3-6`
  - `Melachim II 2:3-3:3`

- Some texts use Talmudic numbering (not just the Talmud); for those, we expect the references in that form.  For example:

  - `Shabbat 7b`
  - `Zohar 2, 20b`  (Refers to volume 2, page 20b)
  - `Rif Shabbat 7b` (Refers to 7b in 'dapei ha Rif')

- You can make Talmud citations more specific by adding segment numbers matching Sefaria's segmentation of the Talmud. These citations will look like `Shabbat 3b:6`, and can also support ranges like `Shabbat 3b:5-11`. You can find Sefaria's segment numbers in the margins of the text on Sefaria. When you click a segment or highlight a range of segments, you will also see the segments reflected in the page URL. When you have a more specific citation, we'll be able to show users more relevant text in the popup, as the links will point to a more precise location. Precise links are prioritized above more general links in the Web Pages section pointing to your site on Sefaria as well.

When the title is interwoven in the text, the linker will fail to catch the reference.  For example:  
`Mishnah Taanit 4:3` would work well, but `In the 3rd Mishnah of Taanit, chapter 4` would fail to match.  The general rule: cite completely, and don't mix anything into the citation.  That will work well for most of the core canon - Tanakh, Mishnah, Tosefta, Talmud, Midrash, Zohar, etc. 

Where it gets trickier is with texts that have a complex structure.  For example, to cite the Four Questions in the Haggadah, this citation would work: `Haggadah, Magid, Four Questions`, as would a number of variants.  However, it is unlikely that a citation like that will occur on its own.  The Mishneh Torah is a similar situation - we record many variants of section names of the Mishneh Torah, but citations are hard to get right without looking at exactly how it is recorded in our system.  A foolproof way of always getting a good reference is to navigate to the text on Sefaria, select the text, and copy the citation from the URL.

### Testing Citations

If you are trying to create a citation in a way that Sefaria can understand, you can use the Sefaria search bar to test. If you type a recognized citation into the search bar and hit enter, it will navigate directly to the text. If it performs a keyword search instead, the citation was not recognized. As you start typing into the search bar, you can use the autocomplete suggestions to see all of the spelling variations that Sefaria understands, which can help you get to the valid citation.

Alternatively, if you can find the text you are citing in Sefaria, the URL will include its citation as Sefaria understands. You will want to replace `_` with ` `, and `.` with `:`.