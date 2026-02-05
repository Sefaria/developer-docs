---
title: Frequently Asked Questions
excerpt: >-
  Looking to get started, but want a rundown of the basics? Here are some common
  questions we get in our inbox — along with some quick answers to help you get
  started.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## How do I retrieve the text of a particular parashah (weekly Torah reading)?

There are two steps involved in retrieving the text of a parashah: 

1. Call the Calendars API. If no date parameters are explicitly passed, this will retrieve the weekly passage read on the week in which you're calling the API.

```python
url = "https://www.sefaria.org/api/calendars"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

A call to the Calendar API will result in a response that looks something like this (truncated):

```json
"calendar_items": \[  
    {  
      "title": {  
        "en": "Parashat Hashavua",  
        "he": "פרשת השבוע"  
      },  
      "displayValue": {  
        "en": "Toldot",  
        "he": "תולדות"  
      },  
      "url": "Genesis.25.19-28.9",  
      "ref": "Genesis 25:19-28:9",
```

2. In the response, note the `ref` field. This field converts the parashah to a ranged text reference corresponding to the verses and chapters of the Five Books of Moses included the relevant Torah portion. Next, use that `ref` field, to query the texts API:

```json
url = "https://www.sefaria.org/api/v3/texts/Genesis 25:19-28:9"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

This query will return the the associated text in the response.

Please note: The text returned by the query will be in Hebrew unless English is explicitly passed.

The same query, with an explicit request for an English response, would look like this:

```python
url = "https://www.sefaria.org/api/v3/texts/Genesis 25:19-28:9?version=english"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

As seen above, the only difference is the addition of the parameter `version=english`. These processes (and more) are detailed in our documentation. To see more, take a look at the description of the parameters and the responses for our [texts API ](ref:get_api-v3-texts-tref).

## How do I retrieve a commentary?

In order to retrieve a commentary on a text in the Sefaria Library, pass the specific commentary to the [Texts API](ref:get_api-v3-texts-tref).

For example, in order to retrieve writings by 11th-century scholar Rashi on the weekly Torah portion referenced above, the query would appear like this:

```python
url = "https://www.sefaria.org/api/v3/texts/Rashi on Genesis 25:19-28:9?version=english"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

Please note: 

* The above query includes the parameter `version=english`, which will return the English-language version of Rashi's writing. This will only work if there is an English-language version of the queried text in the Sefaria Library. 
* It is not currently possible to navigate to a commentary from the API calls to a text. Therefore, the best way to see the available commentaries for a given text is to use our [Related API](ref:get_api-related-tref). For more information on how commentaries are associated with texts, take a look at our documentation. 

## How can I retrieve a range of verses?

This process is the same as the process described above for retrieving the weekly parashah. 

## How can I retrieve a specific parashah along with commentary by Rashi?

This process is the same as the process described above, wherein we passed in `Rashi on Genesis 25:19-28:9` to retrieve Rashi's writings on Parashat Toldot.

## How do I use the Calendar API to retrieve a different weekly Torah portion?

As described in the documentation relating to the [Calendar API ](ref:get_api-calendars-1):

> By default, the API returns for the current time. You can override this default by using a combination of the year, day, and month parameters. All three of these must be used or the API will fallback to the default.

For example, when specifying a different date `January 1st, 2025` by passing the parameters year, month, and day the query would look like this:

```python
url = "https://www.sefaria.org/api/calendars?year=2025&month=1&day=1"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

That will return the ref for the parashah read on the week of January 1st, 2025. Once you have that ref, you can pass it into the Texts API in order to retrieve the text. You can find more information on this process in the above question regarding the retrieval of a specific parashah. 

## How do I retrieve English versions of commentaries that are in Hebrew?

This process is similar to the process described above for retrieving the English-language version of Rashi's commentary on Parashat Toldot. Retrieving the English for a text is as simple as passing the parameter `version=english` on the query.

This is also elaborated on in [our documentation](ref:get_api-v3-texts-tref). Here an except from the relevant paragraph:

> **version** (string)
>
> There are two possible forms for the string passed as the version:
>
> 1. `language`
> 2. `language|versionTitle`
>
> When in the form of `language`, the primary version of that language is returned in the `versions` field of the response object. When in the form of `language|versionTitle`, only that specific version is returned in the `versions` field.
>
> Notes:
>
> * `language` is the full English name of the language. In cases of dialectics with varying sub-specifities, please pass the ‘mother’ language (so for example, `arabic` rather than `judeo-arabic`). This field is NOT case sensitive.
> * `versionTitle` is the exact English `versionTitle `of the given version in the Sefaria database.
> * When only `language` is passed, the response will return a single version of the text in that language, the one that is highest priority in the Sefaria database.
> * Requests can have more than one version param. If no version was passed, the API defaults to `version=primary`.

<br />

An important note - not all of our texts have English translations. In the case where we do not yet have the English translation in our library, it will not appear in the API. To see a list of all English translated texts, [click here](https://www.sefaria.org/translations/en).

## G) How do I get specific commentaries (like Ramban or Rabbi Sacks)?

To retrieve a specific commentary, you follow the steps above in (B). All commentaries are treated as books, so you query the commentary the same way you'd query for the text of any other book.

Here's an example with Ramban on Exodus:

```python
url = "<https://www.sefaria.org/api/v3/texts/Ramban%20on%20Exodus%201.1">

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

With regards to Rabbi Sacks, figuring out the ref to pass is less intuitive. We recommend you navigate to the specific passage of interest in the library, and then copy the ref from the url.

For example, if you wanted [https://www.sefaria.org/Covenant_and_Conversation%3B_Genesis%3B_The_Book_of_the_Beginnings%2C_Bereshit%2C_The_Book_of_Teaching?lang=bi](https://www.sefaria.org/Covenant_and_Conversation%3B_Genesis%3B_The_Book_of_the_Beginnings%2C_Bereshit%2C_The_Book_of_Teaching?lang=bi) via the API, you would pass the request as follows:

```python
url = "<https://www.sefaria.org/api/v3/texts/Covenant_and_Conversation%253B_Genesis%253B_The_Book_of_the_Beginnings%252C_Bereshit%252C_The_Book_of_Teaching">

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)
```

<br />

For more insights and assistance navigating our API, feel free to [contact us](page:contact-us)!
