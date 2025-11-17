---
title: Embed Linker v3 on Your Site
excerpt: >-
  The Sefaria Linker recognizes references on an external web page and links
  them to the corresponding texts on Sefaria
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The Sefaria Linker automatically connects Torah content across the internet to primary sources in our library,                and vice versa. Websites that use the Sefaria Linker give their users direct access on site to any primary sources they cite, allowing curious learners to explore more. The Linker also makes it possible for websites to be automatically linked to from the Sefaria sidebar when our users are exploring texts your site mentions, exposing your site to new learners on Sefaria.

Linker v3 offers advantages for both Hebrew and English content. It decreases incorrect links and catches many more types of citations. For documentation for the previous version, see [Linker v2](doc:linker-v2).

## Bookmarklet

We don't currently have a Javascript bookmarklet available for Linker v3, although you can easily adapt the bookmarklet linked at the bottom of [this page](http://www.sefaria.org/linker) by following this documentation.

## Embed Code

Place this code on your web page, just before the `</body>` tag. These lines will work for a static HTML page. If your site dynamically loads content after the page loads (e.g. Single Page Applications) see the note at the bottom for dynamic sites.

```
	<script type="text/javascript" charset="utf-8" src="https://www.sefaria.org/linker.v3.js"></script>
	<script>
	    sefaria.link();
	</script>
```

## Options

Options can be passed to `sefaria.link()` as key-value pairs in an object.  (See examples below)

### `mode`

The `mode` option affects how the found references behave.  If not specified, the default is "popup-click" mode.

The value of `mode` is one of the following strings:

* `"popup-click"` - when the the user clicks on a reference, a popup is displayed with the textual content.  Within the popup is a link to Sefaria.

* `"link"` - The references are turned into links, which open in a new browser tab when clicked.  There is no popup with textual content.

### `contentLang`

Sets the language used to display the textual content in the popup. Options are `bilingual`, `english`, and `hebrew`. The default value is `bilingual`.

### `interfaceLang`

Sets the language of the popup interface which controls the text in the bottom bar, as well as the orientation (left to right or right to left). Options are `english` or `hebrew`. The default value is `english`.

### `excludeFromLinking`

Sets a CSS selector that defines elements on the page that you do _not_ want to be linked. This may be useful, for example, if you have page headers where links or popups could be distracting. Anything within an existing `<a>` tag will be excluded by default. Citations that are found but excluded are still tracked for usage in the Web Pages section on Sefaria. The value of the parameter is a string which is any valid CSS selector.

### `whitelistSelector`

Sets a CSS selector that defines elements on the page that should always be searched for citations. This field by default shouldn't be necessary to fill in, since the linker relies on heuristics to find the main page content. However, this parameter can be used in cases where part of the page is missed.

### `dynamic`

Set this to true if the linker is being called dynamically-your page content changes dynamically after the initial page load. Normally, the linker sends a tracking signal back to Sefaria and determines the page URL by checking the first canonical link tag on your page. However, most dynamic pages do not update this tag when their content changes. By setting `dynamic `to true, the linker will instead use window.href to get the current URL, ensuring that dynamically generated pages are tracked correctly.

### `popupStyles`

The `popupStyles` option allows for custom styling of the popup box - colors, borders, fonts, etc.  
If present, it must be a dictionary of JavaScript style attributes and their values.  
See examples below.

### `hidePopupsOnMobile`

By default, for a better user experience, the linker won't attempt to create popups on small screens, and instead will fall back to the `link` mode. If you want to use popups on all screen sizes, set this parameter to `false`.

### `versionPreferencesByCorpus`

By default, the popup window will display the default version for each text. Use this parameter to specify preferred versions. If the version title doesn't exist for that text, the default version will be used. You can find version titles available for each book by visiting the book's table of contents and clicking the "Versions" tab. For example, [here](https://www.sefaria.org/Genesis?tab=versions) are the versions for Genesis. Each version preference needs to be organized by language and corpus in the following format:

```
{
   <other linker options>,
   "versionPreferencesByCorpus": {
      <corpus>: {
         <lang>: <version title>
      }
   }
}
```

`corpus` can take one of the following values: `Tanakh`, `Bavli`, `Yerushalmi`, `Mishnah`, `Midrash Rabbah`.

Below is an example usage:

```json
{
   "versionPreferencesByCorpus": {
      "Tanakh": {
         "en": "The Koren Jerusalem Bible"
      },
      "Bavli": {
         "he": "Wikisource Talmud Bavli"
      }
   }
}
```

In this example, "The Koren Jerusalem Bible" version will be used for English for all Tanakh texts. The "Wikisource Talmud Bavli" version will be used for Hebrew for all Bavli texts. In the case that one or both versions do not exist, the defaults will be used.

### `debug`

If true, will turn on debug mode, which may help to determine why a certain citation isn't being linked. In debug mode, citations will be surrounded by a colored border with the following meanings:

* green border: citation successfully linked
* red border: citation recognized but not successfully linked. Either Sefaria doesn't have this text, the citation was incorrectly marked, or the linker has a bug. Note, these citations will not appear when debug mode is false.
* yellow border: citation was linked to multiple possible links because it was determined to be ambiguous. Note, ambiguous citations will not appear when debug mode is off.

To report an issue with a citation in debug mode, click the citation you want to report and click "Report" (this button only appears in debug mode).

## Deprecated options

As of version 3 of the linker, the following options are no longer supported:

* `selector`
* `excludeFromTracking`
* `parenthesesOnly`
* `quotationOnly`

## Dynamic Sites

The linker code searches for citations and creates links once when `sefaria.link()` is called. If you have a dynamic site where content changes after page load, you can make additional calls to `sefaria.link()` any time your content changes. You should pass the parameter `{dynamic: true}` to ensure the linker can recognize the correct URL for the page if it has changed.

## Versions

Now and again we may update the linker to provide additional functionality or modify the interface. If you plan on injecting custom styles, you may prefer to use a specific version of the linker. To do so, simply append the version number to the base file name of the linker. For example,`linker.v3.js`. the unversioned `linker.js` file will always be the most up to date.

## Examples

Include elements with "Torah" class in search for citations:

```
	sefaria.link({whitelistSelector: ".Torah"});
```

Invoke popup-click mode:

```
	sefaria.link({mode: "popup-click"});
```

Add some custom styling to the popup box:

```
	sefaria.link({
		popupStyles: {
			color: "#e3fc11",
			border: '4em solid black' 
		}
	});
```

Do all of the above, together:

```
	sefaria.link({
		mode: "popup-click",
		whitelistSelector: ".Torah",
		popupStyles: {
			color: "#e3fc11",
                	border: '4em solid black' 
		}
	});
```

Use only Hebrew for the interface and content:

```
	sefaria.link({
		contentLang: "hebrew",
		interfaceLang: "hebrew"
	});
```

# Citation Format

As of version 3, the linker should be able to recognize most types of citations. Below are a few examples of the types of citations which should work:

* בראשית א
* רשי ראש השנה ד״ה אלא ביבנה
* פרק מאימתי
* פרק בתרא דפסחים
* שם פרק ג
* בראשית פרק א עד פרק ד

Note, the linker supports ibid / שם citations, although in practice one should be cautious when linking these since they currently cause many false positives.

Use the `debug` option defined above [above](#debug)  to investigate why a citation wasn't linked.
