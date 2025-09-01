---
title: Categories API
excerpt: Retrieving categories via the API.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## The Category API

Sefaria has a Category API which can be found at `sefaria.org/api/category/` .  One can use `GET` requests to look up categories using this endpoint.  

### GET

GET requests take a full category path in the request, e.g. `/api/category/Tanakh/Torah/Genesis`, and return the full category object found.  The above request would return:

```
{
  "path": [
    "Tanakh",
    "Torah"
  ],
    "titles": [
    {
      "lang": "en",
      "text": "Torah",
      "primary": true
    },
    {
      "lang": "he",
      "text": "תורה",
      "primary": true
    }
  ],
  "lastPath": "Torah"
}
```

If the category is not found, the returned object will have an `error` attribute.  If any element of the path is found, the API will return the closest parent in an attribute called `closest_parent`.  This is useful for proactively looking up a category before posting an Index to it. 

For example, a GET request to `/api/category/Tanakh/Torah/Genesis/Bob/Dob` returns:

```
{
  "closest_parent": {
    "path": [
      "Tanakh",
      "Torah"
    ],
    "titles": [
      {
        "lang": "en",
        "text": "Torah",
        "primary": true
      },
      {
        "lang": "he",
        "text": "תורה",
        "primary": true
      }
    ],
    "lastPath": "Torah"
  },
  "error": "Category not found"
}
```

## Creating a New Category

If you decide to get Sefaria up and running locally, you can play around with creating new categories on your own clone of the site. 

In Python: 

```
import django

django.setup()

from sefaria.model import *

c = Category()
c.add_primary_titles("New Category", u"חדשנית")
c.path = ["Existing Parent", "New Category"] # Where "Existing Parent" is an existing parent, such as "Tanakh"
c.save()
```

### POST Request

POST requests need a full valid category object in the `json` attribute of the POST body.  Note that the parent of the category must already exist.  When creating more than one category, the ancestors must be created in order.  

Send a POST request to `/api/category` with a complete serialized category record in the `json` attribute of the POST body.

**Sample`body` of a POST Request to Create a New Category**

```
{
  "apikey": "your_local_api_key", 
	{
  	"path": ["Tanakh", "New Category"],
 		"titles": [
    						{
      	          "lang": "en", 
        	        "primary": "True", 
          	      "text": "New Category"},
  							{
              	  "lang": "he", 
                	"primary": "True", 
                	"text": "חדשנית"}
  						]
	}
}
```

**Note:** When an Index record is posted, the category that it is in must exist.  If the category does not exist, then the Index save will fail with an `InputError` Exception, and an Index post will return a JSON object with an `error` as its only key.

For more on the Category object, see the object [definition](https://github.com/Sefaria/Sefaria-Project/blob/fe9c971217b99cd763c0dd8f42b82339379b961c/sefaria/model/category.py#L14) in `Sefaria-Project/sefaria/model/Category.py`.
