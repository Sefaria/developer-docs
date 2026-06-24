---
title: Embed Linker v2 In Your Site
excerpt: >-
  Learn how to embed Sefaria's powerful citation linking tool into your
  website. 
deprecated: true
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> **Please Note:** [Linker v3](https://developers.sefaria.org/update/docs/linker-v3) offers advantages for both Hebrew and English content by decreasing incorrect links and catching many more types of citations. Currently, Linker v3 is the default version of this tool.

## Embedding Code

In order to embed Linker v2 in your site, place this code on your web page, just before the `</body>` tag. These lines of code will work for a static HTML page. If your site dynamically loads content after the page loads (e.g., Single Page Applications), see [here](#dynamic-sites)

```
	<script type="text/javascript" charset="utf-8" src="https://www.sefaria.org/linker.js"></script>
	<script>
	    sefaria.link();
	</script>
```

## Additional Options

Options that change the behavior of the plugin can be passed in an object to the call to`sefaria.link()`. Here are some available options:

### `selector`

The `selector` option specifies which elements should be checked for references.  The value of the selector is a string — a CSS selector.  You can specify a`#id`, a `.class`, or any other CSS selector.

If a selector is not specified, all of the textual content within the `<body>` tag will be checked for links.

### `mode`

The `mode` option affects how the any references that are found will behave.  If not specified, the default is "pop-up-click" mode.

The value of `mode` is one of the following strings:

* "pop-up-click": When a user clicks on a reference, a pop-up box opens and displays the relevant textual content and a link to the Sefaria Library.

* "link": In this case, any references are turned into links, which open in a new browser tab when clicked, and no pop-up box with textual content appears.

### `contentLang`

This specifies the language used to display the text in the pop-up box. Options are `bilingual`, `english`, and `hebrew`. The default value is `bilingual`.

### `interfaceLang`

This specifies the language of the pop-up interface, which controls both the text in the bottom bar and its orientation (left-to-right or right-to-left). Options are `english` or `hebrew`. The default value is `english`.

### `excludeFromLinking`

This sets a CSS selector that selects elements on the page that you do _not_ want linked. This may be useful, for example, if you have page headers where links or pop-ups may be distracting. Anything within an existing `<a>` tag will be excluded by default. Citations that are found but excluded are still tracked for usage in the Web Pages section in the Sefaria Library, a panel that links Library users to sites across the web that reference a specific text. The value of the parameter is a string, which is any valid CSS selector.

### `excludeFromTracking`

This sets a CSS selector that defines elements on the page that you do _not_ want tracked to appear in the Web Pages section in the Sefaria Library (described above). This may be useful if you have recurring citations on your page that are not specifically relevant to that page in particular. For example, a widget that always shows today's page of Talmud in the ongoing cycle of Daf Yomi study. The value of the parameter is a string, which is any valid CSS selector.

### `quotationOnly`

This option only affects references in Hebrew. It has no effect on English references. If `true`, will only catch Hebrew references that use gershayim in the gematriya part of the reference. Use this option to limit false links created by the linker. A common example of a false positive is דברים לא. This phrase often doesn't refer to Deuteronomy 31. See the table below for examples where `quotationOnly: true` will link:

| Reference     | Will Link? |
| ------------- | ---------- |
| בראשית יב     | NO         |
| בראשית י"ב    | YES        |
| בראשית א      | YES        |
| בראשית י"ב:א  | YES        |
| בראשית י"ב:יא | NO         |

Notice that quotationOnly has no effect when the reference has only one letter in the gematriya (e.g. בראשית א).

### `parenthesesOnly`

Will limit search for references to text in parentheses (`()`) or brackets (`[]`). This is useful if you know references on your site tend to only exist in parentheses or brackets.

### `dynamic`

Set this to true if the linker is being called dynamically-your page content changes dynamically after the initial page load. Normally, the linker sends a tracking signal back to Sefaria and determines the page URL by checking the first canonical link tag on your page. However, most dynamic pages do not update this tag when their content changes. By setting `dynamic `to true, the linker will instead use window.href to get the current URL, ensuring that dynamically generated pages are tracked correctly.

### `popupStyles`

The `popupStyles` option allows for custom styling of the popup box - colors, borders, fonts, etc.  
If present, it must be a dictionary of JavaScript style attributes and their values.  
See examples below.

### `hidePopupsOnMobile`

By default, for a better user experience, the linker won't attempt to create popups on small screens, and instead will fall back to the `link` mode. If you want to use popups on all screen sizes, set this parameter to `false`.

## Dynamic Sites

The linker code searches for citations and creates links once when `sefaria.link()` is called. If you have a dynamic page where content changes after a page load, you can make additional calls to `sefaria.link()` any time your content changes. You should pass the parameter `{dynamic: true}` to ensure the linker can recognize the correct URL for the page if it has changed.

## Versions

Now and then we may update the linker to provide additional functionality or modify the interface. If you plan on injecting custom styles, you may prefer to use a specific version of the linker. To do so, simply append the version number to the base file name of the linker. e.g. `linker.v2.js`. the unversioned `linker.js` file will always be the most up to date.

## Examples of calling the plugin with options

Only look for references in paragraphs with a "Torah" class:

```
	sefaria.link({selector: ".Torah"});
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
		selector: ".Torah",
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

For information on how to format citations so the linker will pick up on them, see:

* [How to Format Citations for the Linker](doc:how-to-format-citations-for-the-linker) (English)
* (Hebrew) [איך ליצור מראי מקורות להפניה](https://developers.sefaria.org/docs/%D7%90%D7%99%D7%9A-%D7%9C%D7%99%D7%A6%D7%95%D7%A8-%D7%9E%D7%A8%D7%90%D7%99-%D7%9E%D7%A7%D7%95%D7%A8%D7%95%D7%AA-%D7%9C%D7%94%D7%A4%D7%A0%D7%99%D7%94)
