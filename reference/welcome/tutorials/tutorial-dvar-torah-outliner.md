---
title: Dvar Torah Outliner
excerpt: >-
  A basic tutorial for beginners looking to get their feet wet with the Sefaria
  API.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: endpoint
      slug: tutorial-tanakh-trivia-game
      title: Tanakh Trivia Game
---
# Dvar Torah Outliner

Let's say you are frequently asked to prepare *Divrei Torah* on the *Parasha* (a short discourse on the weekly Torah portion), and have decided you want to use the Sefaria API to write code to help you automate an outline each week, containing the first verse of that week's *Parasha* along with a sampling of commentaries. 

## Tutorial Set Up

For this tutorial, we'll be using Python 3.8. 

Make sure you have [Python requests](https://pypi.org/project/requests/) installed and imported at the top of your file:

```python
import requests
```

The work we're doing in this tutorial is language agnostic, so feel free to adapt the code to follow along in any language of your choosing.

## Retrieve the *Parasha*

Our first step is to use the [Calendars API](ref:get_api-calendars) to retrieve the weekly *Parasha*. We'll hit the endpoint here without any customizations. If you look at the documentation for this endpoint, you'll see that it is highly customizable based on tradition, location, and cycles of readings. For the sake of this example, we'll stick with the basics. 

```python
url = "https://www.sefaria.org/api/calendars"

# Make a GET request to the URL
response = requests.get(url)

# Parse the response as JSON
data = response.json()  
```

If you `print` the value of `data`, you'll see a dictionary with lots of information about that day's learning schedules (i.e. Daf Yomi, Daily Rambam... and weekly *Parasha*). 

```json
{
  "date": "2024-02-06",
  "timezone": "America/Halifax",
  "calendar_items": [
    {
      "title": 
      {
        "en": "Parashat Hashavua",
        "he": "פרשת השבוע"
      },
      "displayValue": {
        "en": "Mishpatim",
        "he": "משפטים"
      },
      "url": "Exodus.21.1-24.18",
      "ref": "Exodus 21:1-24:18",
      "heRef": "שמות כ״א:א׳-כ״ד:י״ח",
      "order": 1,
      "category": "Tanakh",
      "extraDetails": {
        "aliyot": [
          "Exodus 21:1-21:19",
          "Exodus 21:20-22:3",
          "Exodus 22:4-22:26",
          "Exodus 22:27-23:5",
          "Exodus 23:6-23:19",
          "Exodus 23:20-23:25",
          "Exodus 23:26-24:18",
          "Numbers 28:9-28:15"
        ]
      },
      "description": {
        "en": "Mishpatim (“Laws”) recounts a series of God’s laws that Moses gives to the Israelites. These include laws about treatment of slaves, damages, loans, returning lost property, the Sabbath, the sabbatical year, holidays, and destroying idolatry. The portion ends as Moses ascends Mount Sinai for 40 days.",
        "he": "פרשת משפטים כוללת מספר גדול של דינים ומצוות שאלוהים מצווה את בני ישראל באמצעות משה. הדינים עוסקים בתחומים רבים, כגון עבדות, נזיקין, השבת אבדה, שבת, שמיטה, חגים, איסור עבודה זרה ועוד. בסוף הפרשה מתוארים מעמד כריתת הברית בין אלוהים לעם ועליית משה להר סיני ל40 יום ו40 לילה."
      }
    },
    {
      "title": {
          "en": "Haftarah",
          "he": "הפטרה"
      },
      "displayValue": {
          "en": "Isaiah 66:1-24",
          "he": "ישעיהו ס״ו:א׳-כ״ד"
      },
      "url": "Isaiah.66.1-24",
      "ref": "Isaiah 66:1-24",
      "order": 2,
      "category": "Tanakh"
      },
      {
      "title": {
          "en": "Daf Yomi",
          "he": "דף יומי"
      },
      "displayValue": {
          "en": "Bava Kamma 96",
          "he": "בבא קמא צ״ו"
      },
      "url": "Bava_Kamma.96",
      "ref": "Bava Kamma 96",
      "order": 3,
      "category": "Talmud"
      },
   ...
	]
}
```

We're specifically interested in the list of items stored in the array under the field `calendar_items`, and we want the data in the dictionary with the `title` of `Parashat Hashavua`, so let's retrieve that. 

What's most interesting to us within the `Parashat Hashavua` data is the `ref`, the specific Sefaria citation for this text. (To read more about our citation system, see [Text References](doc:text-references). It is a core aspect of working with our API). 

```python
# Retrieve the list of calendar_items
calendar_items = data['calendar_items']

# Find the dictionary in calendar_items storing the metadata for Parashat Hashavua, 
# and save the ref, as well as the name of the week's Parasha.  
for item in calendar_items:
    if item['title']['en'] == 'Parashat Hashavua':
        parasha_ref = item['ref'] 
        parasha_name = item['displayValue']['en']
```

Great! Now we have our citation for the specific week's *Parasha*. This ref will appear as a range, so for example, for the week of *[Parashat Mishpatim](https://www.sefaria.org/topics/parashat-mishpatim?sort=Relevance\&tab=sources)* , printing `parasha_ref` returns `Exodus 21:1-24:18`, and printing `parasha_name` returns `Mishpatim`. 

```python
print(parasha_ref) # The ranged citation for the week's Parasha
print(parasha_name) # The name of the Parasha
```

## Selecting the First Verse

For this specific tutorial, we want to limit our *Dvar Torah* to only the first verse from this week's *Parasha*. To do this, we will collect the first citation in the range; in the case of `Exodus 21:1-24:18`, we want only `Exodus 21:1`. 

```python
parasha_ref = parasha_ref.split("-")[0]
print(parasha_ref) # The first verse of this week's parasha
```

**Please Note:** This is not a requirement for the subsequent API calls. We are intentionally limiting the breadth of our text to a specific verse for the sake of the example. Please feel free to play with these calls using text citations (`refs`) that span multiple chapters.

## Retrieving the Parasha Text

Now that we have our `parasha_ref` all set to contain a string citing the first verse in this week's *Parasha*, let's make another API call to the [Texts (v3)](ref:get_api-v3-texts-tref-1) API to retrieve the text of that verse. 

```python
url = f"https://www.sefaria.org/api/v3/texts/{parasha_ref}"

# Make GET request
response = requests.get(url)

# Parse response as JSON 
data = response.json() 
```

When we print `data`, we get a massive dictionary containing lots of data regarding the various editions of this text on Sefaria. Below is a truncated version of what's returned (notice the `...` replacing data irrelevant for this tutorial). 

```json
{
  "versions": [
    {
      "status": "locked",
      "priority": 2,
      "license": "CC-BY-SA",
      "versionNotes": "<i>Miqra According to the Masorah</i> (MAM) is a digital Hebrew edition of the Tanakh based on the Aleppo Codex and related manuscripts. It is designed for readers, and as such it contains added elements to aid vocalization of the text. For instance: When an accent is marked in an unstressed syllable, an extra accent is added in the proper place (<i>pashta</i>, <i>zarqa</i>, <i>segol</i>, <i>telisha</i>). <i>Legarmeih</i> and <i>paseq</i> are visibly distinguished. <i>Qamaz qatan</i> is indicated by its designated Unicode character (alternatives are documented where traditions differ about its application).<br>The text of MAM is fully documented. The <a href=\"https://he.wikisource.org/wiki/%D7%9E%D7%A9%D7%AA%D7%9E%D7%A9:Dovi/%D7%9E%D7%A7%D7%A8%D7%90_%D7%A2%D7%9C_%D7%A4%D7%99_%D7%94%D7%9E%D7%A1%D7%95%D7%A8%D7%94/%D7%9E%D7%99%D7%93%D7%A2_%D7%A2%D7%9C_%D7%9E%D7%94%D7%93%D7%95%D7%A8%D7%94_%D7%96%D7%95\">complete introduction</a> to the edition (Hebrew) explains the types of editorial decisions that have been made and the reasons for them (<a href=\"https://en.wikisource.org/wiki/User:Dovi/Miqra_according_to_the_Masorah#About_this_Edition_(English_Abstract)\">English abstract</a>). In addition, every word in the Bible about which there is some textual concern or ambiguity includes a documentation note; these notes can be viewed conveniently <a href=\"https://bdenckla.github.io/MAM-with-doc/\">here</a>. If an error is discovered, it may be reported to <a href=\"https://he.wikisource.org/wiki/%D7%A9%D7%99%D7%97%D7%AA_%D7%9E%D7%A9%D7%AA%D7%9E%D7%A9:Dovi\">User:Dovi</a> at Hebrew Wikisource. Please check the documentation notes before reporting an error.",
      "versionTitleInHebrew": "מקרא על פי המסורה",
      "actualLanguage": "he",
      "languageFamilyName": "hebrew",
      "isPrimary": true,
      "direction": "rtl",
      "language": "he",
      "versionSource": "https://he.wikisource.org/wiki/%D7%9E%D7%A9%D7%AA%D7%9E%D7%A9:Dovi/%D7%9E%D7%A7%D7%A8%D7%90_%D7%A2%D7%9C_%D7%A4%D7%99_%D7%94%D7%9E%D7%A1%D7%95%D7%A8%D7%94",
      "versionTitle": "Miqra according to the Masorah",
      "text": "וְאֵ֙לֶּה֙ הַמִּשְׁפָּטִ֔ים אֲשֶׁ֥ר תָּשִׂ֖ים לִפְנֵיהֶֽם׃",
      /**Additional attributes omitted for the sake of the example**/
    }
  ],
  "available_versions": [...]
}
```

Inside the object inside the `versions` array, we can see a field called `text` storing the text of this verse. Let's save that to a variable, and also grab the `versionTitle` of this edition of the text. Since this edition of the text is in Hebrew, we'll name the variables `he_vtitle` and `he_pasuk`.

```python
he_vtitle = data['versions'][0]['versionTitle'] 
he_pasuk = data['versions'][0]['text']
```

Big progress! We have our verse for the *Dvar Torah* outline; let's move on to the next step. 

## Retrieving a Different Edition of the Text

While the Hebrew verse is useful, let's also retrieve an English translation of the verse. You can query the `api/v3/texts` endpoint for many different editions of the text in different languages, or different versions within a language. For now, we'll just repeat what we did above, making the same GET request, but adding the query parameter `version=english`. (To learn more about all of our available translations, how to request specific versions, and all of the customizations, see [Texts (v3)](ref:get_api-v3-texts-tref-1)). 

```python
# Change version to English
url = f"https://www.sefaria.org/api/v3/texts/{parasha_ref}?version=english"

# Make the GET request
response = requests.get(url)

# Parse the response as JSON
data = response.json()

# Retrieve the version title, and text for the English verse.
en_vtitle = data['versions'][0]['versionTitle']
en_pasuk = data['versions'][0]['text']
```

## Retrieving Parasha Commentaries

Fantastic progress! We have our Hebrew text of the verse, and our English text of the verse. Now we just need some commentaries to build out the rest of this outline. 

To retrieve our commentaries, we're going to use the [Related](ref:get_api-related-tref) API endpoint, and pass in our `parasha_ref` as the `tref` (i.e. **t**ext **ref**erence). 

```python
url = f"https://www.sefaria.org/api/related/{parasha_ref}"

# Make the GET request
response = requests.get(url)

# Parse the response as JSON
data = response.json()
```

When we print `data`, we get a dictionary that has the following keys:

```json
{
  "links": [...],
  "sheets": [...],
  "notes": [...],
  "webpages": [...],
  "topics": [...],
  "manuscripts": [...],
  "media": [...]
}
```

Each key points to an array containing a list of JSON objects, each representing a connection between our query text (`parasha_ref`) and connections of that type to other items in the library. 

For example, a sample JSON object from the `links` array (containing all links between our query text and other linked texts) will look like this:

```json
{
  "_id": "51611f84edbab4794f78309b",
  "index_title": "Shemot Rabbah",
  "category": "Midrash",
  "type": "midrash",
  "ref": "Shemot Rabbah 30:2",
  "anchorRef": "Exodus 21:1",
  "anchorRefExpanded": ["Exodus 21:1"],
  "sourceRef": "Shemot Rabbah 30:2",
  "sourceHeRef": "שמות רבה ל׳:ב׳",
  "anchorVerse": 1,
  "sourceHasEn": true,
  "compDate": [1200],
  "commentaryNum": 30.0002,
  "collectiveTitle": {
    "en": "Shemot Rabbah",
    "he": "שמות רבה"
  },
  "heTitle": "שמות רבה"
}
```

This is a link between our `parasha_ref` to the [Midrash](https://www.sefaria.org/texts/Midrash) of *Shemot Rabbah*. You'll notice we're getting a lot of metadata here, but most critical for our purposes is the `type` of link, as well as the `ref` for the linking text. 

The `type` field tells us the general type of connection between our `parasha_ref` and the text returned in the `ref` field. Some possible link types may include `targum`, `reference`, `quotation`, `commentary`, `essay`, `midrash` or `parshanut` among others. 

For sake of simplicity, in this tutorial, let's only look at linked texts with a `type` of `commentary`. 

We'll iterate through the `links` array in the response `data`, and append the `ref` (the citation) of all linked texts of type `commentary` to a list we'll call `commentaries`. 

```python
# Set up our commentaries list
commentaries = []

# Iterate through the "links" in our data response, filtering for commentary links, and 
# appending the text reference to our commentaries list. 
for linked_text in data["links"]:
    if linked_text['type'] == 'commentary':
        commentaries.append(linked_text['ref'])
```

Great! Now you have a list of all commentary links to the first verse in this week's *Parasha*. We are almost there!

## Retrieving the Commentary Text

Our next step is to make additional calls to the [Texts (v3)](ref:get_api-v3-texts-tref-1) endpoint to retrieve the text of our commentaries. In theory, you also do some filtering here to determine exactly which commentaries you'd like in your outline. 

For the sake of the brevity of this tutorial, we will just include the first three commentaries that appear in our list in the outline. Feel free to play with this! There's a lot that can be done at this stage. 

Since we'll be repeating the same process three times, let's define a function `get_commentary_text(ref)` to help us with our text retrieval:

```python
def get_commentary_text(ref):
  	
    # Our standard GET request, and JSON parsing
    url = f"https://www.sefaria.org/api/v3/texts/{ref}"
    response = requests.get(url)
    data = response.json()

    # Checking to make sure we have a primary version for the commentary 
    # (otherwise, the versions field would be empty). 
    if "versions" in data and len(data['versions']) > 0:
      
      # Retrieve the general name of the commentary book (i.e. "Rashi on Genesis")
      title = data['title']
      
      # Retrieve the text of the commentary
      text = data['versions'][0]['text']

      # Return the title, and the text
      return title, text
```

Now, we'll apply this function to the first three commentaries in our list of `commentaries`:

```
com1_title, com1_text = get_commentary_text(commentaries[0])
com2_title, com2_text = get_commentary_text(commentaries[1])
com3_title, com3_text = get_commentary_text(commentaries[2])
```

Congratulations! We are ready for the final step, printing our outline!

## Printing your Outline

At this point, we should have the following variables defined in our code:

* `parasha` - The name of the *Parasha* (i.e. *Mishpatim,\_Eikev* etc)
* `parasha_ref` - The specific text reference for the first verse of that *Parasha* (i.e. `Exodus 21.1`)
* `he_vtitle`- The version title for the specific Hebrew edition of the text of the verse
* `he_text` - The Hebrew text for the verse
* `en_vtitle` - The version title for the specific English edition of the text of the verse
* `en_text` - The English text for the verse
* And lastly, as seen in the code above, the commentary title and commentary text for the first three commentaries in our `commentaries` list. 

Now that we have everything in place, let's print it out:

```python
print(f"My Outline for Parashat {parasha_name}")
print(f"Hebrew: {he_pasuk} (edition: {he_vtitle})")
print(f"English: {en_pasuk} (edition: {en_vtitle})")
print("\nThree Commentaries:")
print(f"A) {com1_title}: {com1_text}\n")
print(f"B) {com2_title}: {com2_text}\n")
print(f"C) {com3_title}: {com3_text}")
```

When we ran this code the week of *Parashat Mishpatim*, this is what printed:

```
My Outline for Parashat Mishpatim
Hebrew: וְאֵ֙לֶּה֙ הַמִּשְׁפָּטִ֔ים אֲשֶׁ֥ר תָּשִׂ֖ים לִפְנֵיהֶֽם׃ (edition: Miqra according to the Masorah)
English: These are the rules that you shall set before them: (edition: The Contemporary Torah, Jewish Publication Society, 2006)

Three Commentaries:
A) Ramban on Exodus 21:1: טעם <b>ואלה המשפטים אשר תשים לפניהם</b>, כי רצה להקדים להם המשפטים, כי כאשר היה בעשרת הדברות הדבור הראשון בידיעת ה', והשני באיסור ע"ז, חזר וצוה את משה <b>כה תאמר אל בני ישראל אתם ראיתם כי מן השמים דברתי עמכם</b> (שמות כ׳:כ״ב), שתזהירם אתה עוד שיתנו לבם למה שראו ויזהרו במצות האלו שצויתים, <b>כי אתם ראיתם</b>, כנגד דבור <b>אנכי</b>, <b>ולא תעשון אתי</b> כנגד <b>לא יהיה לך</b>, להשלים ענין ע"ז, <b>ואלה המשפטים</b> כנגד <b>לא תחמוד</b>, כי אם לא ידע האדם משפט הבית או השדה ושאר הממון יחשוב שהוא שלו ויחמדהו ויקחהו לעצמו, לפיכך אמר <b>תשים לפניהם</b>, משפטים ישרים ינהיגו אותם ביניהם, ולא יחמדו מה שאינו שלהם מן הדין וכן אמרו במדרש רבה (שמות ל טו) כל התורה כלה תלויה במשפט, לכן נתן הקב"ה דינין אחר עשרת הדברות. וכן יפרש באלה המשפטים המשפט בעבודה זרה (שמות כ״ב:י״ט), ובכבוד האב (שם כא טו יז), והרציחה (שם כא יב יד), והניאוף (שם כב יח), הנזכרים בעשרת הדברות:  <br><b>ודרשו</b> לפניהם ולא לפני כנענים (תנחומא א, גיטין פח:), מפני שהיה ראוי לומר אשר תשים להם, כמו שאמר (שמות ט״ו:כ״ה) שם שם לו חק ומשפט, ואמר לפניהם, שהם יהיו הדיינין, כי על השופט יבא הלשון הזה, ועמדו שני האנשים אשר להם הריב לפני ה' לפני הכהנים והשופטים (דברים יט יז), עד עמדו לפני העדה למשפט (במדבר לה יב), לפני כל יודעי דת ודין (אסתר א יג) ודרשו עוד (שם) לפניהם, ולא לפני הדיוטות, מפני שכתב במשפטים והגישו אדוניו אל האלהים (שמות כ״א:ו׳), עד האלהים יבא דבר שניהם (שמות כ״ב:ח׳), וכתוב גם כן ונתן בפלילים (שמות כ״א:כ״ב), שהם הדיינין המומחין הסמוכים עד משה רבינו:  <b>ולכך</b> אמר בכאן שהמשפטים האלה ישימו אותם לפני האלהים שיזכיר, ולא לפני גוים, ולא לפני מי שאינו שופט על פי התורה, והוא הדיוט לזה, שאסור לבא בפניו כשם שאסור לבא לפני הגוים ואע"פ שידוע שההדיוט הזה יודע שורת הדין וידין לו כהוגן, אבל הוא אסור לשומו דיין ולצעוק לו שיכוף את בעל דינו לדון לפניו, וההדיוט עצמו אסור לדון להם. ואף על פי שהזכירו חכמים שתי הכתות האלה כאחת, יש הפרש ביניהם, שאם רצו שני בעלי הדין לבא לפני ההדיוט שבישראל מותר הוא, ובדקבלום עילויהו דינו דין, אבל לפני הגוים אסורין הם לבא לפניו שידון להם בדיניהם לעולם, ואפילו היו דיניהם כדיננו באותו ענין:  

B) Kli Yakar on Exodus 21:1: <b>ובזה מיושב ג"כ,</b> מה היה הצורך הגדול לצות להם על עשיית המזבח בשלמא הדינין היה צורך שעה אבל המזבח לא היה כל כך צורך שעה כי עדיין לא נצטוו על המשכן, אלא ודאי לכך הקדים מלאכת המזבח אל פר' משפטים לומר לך שתשים הסנהדרין אצל המזבח כדי שילמדו מן המזבח שכל מה שפוסל במזבח פוסל גם בסנהדרין.  

C) Mizrachi, Exodus 21:1: <b>לפניהם ולא לפני כותים כו'. </b>בגטין פרק המגרש ופרש"י דלפניהם קא אע' זקנים שעלו עמו להר קודם מתן תורה כדכתיב ואל משה אמר עלה וגו' פירוש שהם כלם סמוכים ומיקרו אלהים ואימעוט מלפניהם אפילו ישראלים הדיוטות וכל שכן כותים א"נ קרא דלפניהם אהאלהים דכתיב בפרשה קאי כדפריך ואם תאמר הא מלפניהם דרשינן לעיל שחייב להראות לו פנים או כשלחן הערוך ומוכן לפני האדם כעניין שנאמר אתה הראת לדעת יש לומר מדה"ל למכתב אשר תשים לפניהם ש"מ למעוטי כותים ומה שכתב הרמב"ן ודרשו לפניהם ולא לפני כותים מפני שהיה ראוי לומר אשר תשים להם כמו שאמר שם שם לו חק ומשפט ואמר לפניהם שהם יהיו הדיינים כי על השופט יבא הלשון הזה ועמדו שני האנשים אשר להם הריב לפני הכהנים והשופטים עד עמדו לפני העדה למשפט לפני כל יודעי דת ודין אינו מחוור כי כבר דרשו הלשון מעניין פנים ואפשר שהוא סובר שקול הוא ויבאו שניהם והא דנקט ולא לפני כותים ולא נקט ולא לפני הדיוטות כדתניא בפרק המגרש וכל שכן ולא לפני כותים משום דמיעוטא דהדיוטות אינו דומה למיעוטא דכותים דאף על גב דתרויהו מקרא דלפניהם קא ממעטי להו בבריתא דהמגרש מ"מ יש הפרש ביניהם דמיעוטא דלפניהם ולא לפני הדיוטות אינו אלא דלא מצו לאכפויי לבעלי הריב לבא להדיין לפניהם אבל אם רצו לבא לפניהם מותר ודיניהם דין ואלו מעוטא דלפניהם ולא לפני כותים אפילו אם ירצו לבא לפניהם להדיין בדיניהם אסור והיינו דמסיים בה שהמביא דיני ישראל לפני כותים מחלל את השם ומיקר שם עבוד' כוכבים והא דקאמר ואפי' ידעת בדין אחד שכותים דנין אותו כדיננו אינו רוצה לומר דממיעוט' דלפניהם ולא לפני כותים שמעינן בין בדנין כדיננו בין באין דנין כדיננו דבאין דנין כדיננו לא צריך קרא למיעוטא דתפוק לי משום גזל אלא הכי פירושה לא מבעיא בשאין דנין כדיננו דההוא תפיק לי משום גזל אלא אפילו בשדנין כדיננו אל תביאהו בערכאות שלהם ממיעוטא דלפניהם ולא לפני כותים אי נמי אפילו באין דנין כדיננו אצטרך קרא למיעוטא דסד"א דינא דמלכותא דינא כדאיתא בפרק קמא   דגיטין ואין כאן משום גזל: 

```

<Callout icon="🎉" theme="default">
  ### Congratulations! You have officially written a script to generate a *D'var Torah* outline for the weekly *Parasha*!
</Callout>

 We invite you to dive deeper into our API, and all of the infinite possibilities with our data in the documentation. Looking forward to seeing what you can build!

## The Full Code

The full code for the script can be seen below. The code has been slightly refactored and reorganized for better readability and code reuse within this script. The stages are marked in the comments. 

```python
import requests


# Helper functions

def get_request_json_data(endpoint, ref=None, param=None):
    url = f"https://www.sefaria.org/{endpoint}"

    if ref:
        url += f"{ref}"

    if param:
        url += f"?{param}"

    response = requests.get(url)
    data = response.json()
    return data


def get_commentary_text(ref):
    data = get_request_json_data("api/v3/texts/", ref)

    # Checking to make sure we have a primary version for the commentary
    # (otherwise, the versions field would be empty).
    if "versions" in data and len(data['versions']) > 0:
        # Retrieve the general name of the commentary book (i.e. "Rashi on Genesis")
        title = data['title']

        # Retrieve the text of the commentary
        text = data['versions'][0]['text']

        # Return the title, and the text
        return title, text


if __name__ == '__main__':

    # Step One: Get the Parasha Data Using the Calendars API
    data = get_request_json_data("api/calendars")

    # Retrieve the list of calendar_items
    calendar_items = data['calendar_items']

    # Find the dictionary in calendar_items storing the metadata for Parashat Hashavua,
    # and save the ref, as well as the name of the week's Parasha.
    for item in calendar_items:
        if item['title']['en'] == 'Parashat Hashavua':
            parasha_ref = item['ref']
            parasha_name = item['displayValue']['en']

    ##################################################################

    # Step Two: Split the Ranged Ref to get the First Verse
    parasha_ref = parasha_ref.split("-")[0]

    ##################################################################

    # Step Three: Retrieve Text for the Verse
    data = get_request_json_data("api/v3/texts/", parasha_ref)
    he_vtitle = data['versions'][0]['versionTitle']
    he_pasuk = data['versions'][0]['text']

    # Change version to English - Retrieve the version title, and text for the English verse.
    data = get_request_json_data("api/v3/texts/", parasha_ref, "version=english")
    en_vtitle = data['versions'][0]['versionTitle']
    en_pasuk = data['versions'][0]['text']

    ##################################################################

    # Step Four: Get Commentaries
    data = get_request_json_data("api/related/", parasha_ref)

    # Set up our commentaries list
    commentaries = []

    # Iterate through the "links" in our data response, filtering for commentary links, and
    # appending the text reference to our commentaries list.
    for linked_text in data["links"]:
        if linked_text['type'] == 'commentary':
            commentaries.append(linked_text['ref'])

    com1_title, com1_text = get_commentary_text(commentaries[0])
    com2_title, com2_text = get_commentary_text(commentaries[1])
    com3_title, com3_text = get_commentary_text(commentaries[2])

    ##################################################################

    # Step Five: Print Parasha Outline

    print(f"My Outline for Parashat {parasha_name}")
    print(f"Hebrew: {he_pasuk} (edition: {he_vtitle})")
    print(f"English: {en_pasuk} (edition: {en_vtitle})")
    print("\nThree Commentaries:")
    print(f"A) {com1_title}: {com1_text}\n")
    print(f"B) {com2_title}: {com2_text}\n")
    print(f"C) {com3_title}: {com3_text}")

```

**Note**: The functions in this script were not written to par with adequate testing and `try/except` blocks that you would expect for more robust API calls. We intentionally wrote code that was as simple and bare-bones as possible to allow beginners to focus on a first attempt at using the API to retrieve data. We hope this is a helpful first step!
