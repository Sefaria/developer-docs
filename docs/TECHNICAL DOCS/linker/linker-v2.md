---
title: Linker v2
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
## Embed Code

Place this code on your web page, just before the `</body>` tag. These lines will work for a static HTML page. If your site dynamically loads content after the page loads (e.g. Single Page Applications) see [here](#dynamic-sites)

```
	<script type="text/javascript" charset="utf-8" src="https://www.sefaria.org/linker.js"></script>
	<script>
	    sefaria.link();
	</script>
```

## Options

Options which can change the behavior of the plugin can be passed in an object to the call to`sefaria.link()`. Here are some available options:

### `selector`

The `selector` option is used to specify which elements should be checked for references.  The value of selector is a string - a CSS selector.  You can specify an `#id , a .class` , or any other CSS selector.  

If a selector is not specified, all of the textual content within the `<body>` tag is checked for links. 

### `mode`

The `mode` option affects how the found references behave.  If not specified, the default is "popup-click" mode.

The value of `mode` is one of the following strings:

* "popup-click" - when the the user clicks on a reference, a popup is displayed with the textual content.  Within the popup is a link to Sefaria.  

* "link" - The references are turned into links, which open in a new browser tab when clicked.  There is no popup with textual content. 

### `contentLang`

Sets the language used to display the textual content in the popup. Options are `bilingual`, `english`, and `hebrew`. The default value is `bilingual`.

### `interfaceLang`

Sets the language of the popup interface which controls the text in the bottom bar, as well as the orientation (left to right or right to left). Options are `english` or `hebrew`. The default value is `english`.

### `excludeFromLinking`

Sets a CSS selector that defines elements on the page that you do *not* want to be linked. This may be useful, for example, if you have page headers where links or popups could be distracting. Anything within an existing `<a>` tag will be excluded by default. Citations that are found but excluded are still tracked for usage in the Web Pages section on Sefaria. The value of the parameter is a string which is any valid CSS selector.

### `excludeFromTracking`

Sets a CSS selector that defines elements on the page that you do *not* want tracked to be displayed on the Web Pages section on Sefaria. This may be useful if you have recurring citations on your page that are not specifically relevant to that page in particular. For example, a widget that always shows today's daf in Daf Yomi. The value of the parameter is a string which is any valid CSS selector.

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

Set this to true if the linker is being called dynamically-your page content changes dynamically after the initial page load. Normally, the linker sends a tracking signal back to Sefaria and determines the page URL by checking the first canonical link tag on your page. However, most dynamic pages do not update this tag when their content changes. By setting `dynamic `to true, the linker will instead use window\.href to get the current URL, ensuring that dynamically generated pages are tracked correctly.

### `popupStyles`

The `popupStyles` option allows for custom styling of the popup box - colors, borders, fonts, etc.\
If present, it must be a dictionary of JavaScript style attributes and their values.\
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
