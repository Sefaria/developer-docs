---
title: Tanakh Trivia Game
excerpt: >-
  This tutorial demonstrates a proof-of-concept for a Tanakh trivia game powered
  by the Sefaria API, in just about 100 lines of code.
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
      slug: text
      title: Text
---
# Tanakh Trivia

Let's work together to build a mini Tanakh Trivia Game in fewer than 100 lines of code. We'll hit multiple endpoints in the Sefaria API, and weave it all together for an educational activity in the console.

## Project Outline

Before we start coding, let's go over the game structure, and which endpoint(s) each trivia question requires. 

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        \# 
      </th>

      <th>
        Question
      </th>

      <th>
        Endpoint(s)
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1
      </td>

      <td>
        How many chapters does `<Book of Tanakh>` have?
      </td>

      <td>
        [Shape](ref:get_api-shape-title) API
      </td>
    </tr>

    <tr>
      <td>
        2
      </td>

      <td>
        Which chapter of that `<Book of Tanakh>` has the fewest verses?
      </td>

      <td>
        [Shape](ref:get_api-shape-title)  API
      </td>
    </tr>

    <tr>
      <td>
        3
      </td>

      <td>
        Select a verse from `<Book of Tanakh>`. Which topics might be connected to this text?
      </td>

      <td>
        [Texts (v3)](ref:get_api-v3-texts-tref) API\
        *and*\
        [Ref-Topic-Links](ref:get_api-ref-topic-links-tref) API
      </td>
    </tr>
  </tbody>
</Table>

We have chosen to limit the user's selection of Books of Tanakh to three specific books to provide a working sample. In a full scale edition of this project, the user would be able to select any book of their choosing. 

:warning: It is important to note, this is a proof-of-concept. We take some shortcuts and stick to this limited question set on purpose, in order to showcase how someone can use our API for projects like these. We hope you are inspired, and take your work to the next level! 

## Set Up

For this tutorial, we'll be using Python 3.8. 

Make sure you have [Python requests](https://pypi.org/project/requests/) installed and imported at the top of your file:

```python
import requests
```

The work we're doing in this tutorial is language agnostic, so feel free to adapt the code to follow along in any language of your choosing.

## Select a Book

The first thing we are going to do is prompt the user to select a Book of Tanakh to be their focus for all of the trivia questions to follow. 

```python
print("Welcome to the Tanakh Trivia Game!")

# Setting up the 'score' variable
score = 0

# Limited choices for the user
index_dict = {"A": "Exodus", "B": "Joshua", "C": "Esther"}

# Printing the menu
for key in index_dict:
  print(f"Option {key}: {index_dict[key]}")
  s = input("Select a text from the list by pressing the corresponding letter key ['A', 'B', 'C']: ").upper()

```

The user will see something like this in their console:

```
Welcome to the Tanakh Trivia Game!
Option A: Exodus
Option B: Joshua
Option C: Esther
Select a text from the list by pressing the corresponding letter key ['A', 'B', 'C']:
```

Our next step is to process the user input:

```python
# If they selected a valid key, set the book for the game
if s in index_dict.keys():
  print(f"You picked {index_dict[s]}")
  index_title = index_dict[s]
  
else:
  print("Sorry! That's not a valid option. Restart the game and try again.")
```

## Shape API

Our first two questions rely heavily on the [Shape](ref:get_api-shape-title) API. The [Shape](ref:get_api-shape-title) API returns data about the structure of a book on Sefaria. For example, if someone were to query the [Shape](ref:get_api-shape-title) API for the book of `Esther`, they'd get the following JSON in return:

```json
{
	"section": "Writings",
	"heTitle": "אסתר",
	"title": "Esther",
	"length": 10,
	"chapters": [
		22,
		23,
		15,
		17,
		14,
		14,
		10,
		17,
		32,
		3
	],
	"book": "Esther",
	"heBook": "אסתר"
}
```

The most interesting data here appears in the `length` and the `chapters` fields. The `length` field tells you the 'length' of the text in chapters (in this case, Esther has 10 chapters). The `chapters` field that contains integers. There are `length` number of integers in a given book of Tanakh, so in the case of Esther, we have 10 integers in this `chapters` array. Each position in the array corresponds with a chapter, and each value corresponds to the number of verses. We see here that Chapter One has 22 verses, Chapter Five has 14 verses, and Chapter Nine has 32 verses.

We're going to use the [Shape](ref:get_api-shape-title) API to ask some questions about the structure of a book.

## Let's start Trivia-ing

First, let's query the [Shape](ref:get_api-shape-title) API:

```python
url = f"https://www.sefaria.org/api/shape/{index_title}"
response = requests.get(url)
shape = response.json()
```

Now, let's see if the player can guess the number of chapters in the book. 

We'll grab that data, and save it as our `answer`. 

```python
answer = shape[0]["length"]
```

And now we'll prompt the user with the question. Since we want all responses to this question to be integers, we'll cast their input as an `int()`. We do this inside a `try`/`except` block.

```python
q1 = input(f"How many chapters are in {index_title}? ")

try:
  q1 = int(q1)
except Exception as e:
  print("Sorry, that's not a number. Please restart the game and try again")
```

Assuming we received a numeric response, let's check it against our data in `answer` from the [Shape](ref:get_api-shape-title) API to see whether or not they guessed correctly:

```python
if answer == q1:
  print("Yes! You got it right!")
  score += 1
else:
  print(f"Sorry! That's incorrect. The correct answer was: {answer}. You answered {score}/3 question(s)")
```

While playing, the user will see something like this:

```
You picked Esther
Here are some trivia questions about that text:
How many chapters are in Esther? 
>>> 10
Yes! You got it right!
```

## Time For Another Question

Let's ask the user another question using data from the [Shape](ref:get_api-shape-title) API again. Recall, our API response is stored inside a variable called `shape`. 

First, let's calculate the shortest chapter of the book. We'll use the Python `min()` function on the chapters array to find the position with the fewest verses. Then (since our array is 0-based) we'll add `1` to calculate a human-readable chapter. 

```python
q2 = input(f"Which chapter of {index_title} is the shortest? ")

shortest = min(shape[0]["chapters"]) + 1
```

Now that we've prompted the user and calculated the answer, we'll follow a similar `try`/`except` pattern like before to check if the user's answer is correct, and inform them accordingly:

```python
try:
  q2 = int(q2)
except Exception as e:
  print("Sorry, that's not a number. Please restart the game and try again")

if shortest == q2:
  print("Yes! You got it right!")
  score += 1
else:
  print(f"Sorry! That's incorrect. The correct answer was: {shortest}. You answered {score}/3 question(s)")

```

Once again, on the user's side, they might see something like this:

```
Which chapter of Joshua is the shortest? 
>>> 23
Sorry! That's incorrect. The correct answer was: 24. You answered 1/3 question(s)
```

## Last Round

Alrighty, let's move on to one final question. For this question, we'll use the [Ref-Topic-Links](ref:get_api-ref-topic-links-tref) API to prompt the user to guess which topics might be related to a given verse. 

Our first step is to prompt the user to provide us with a specific reference to a verse. (Note: if this were a "real" game, and not just a tutorial we'd need to add some data validation here - but for the purposes of a tutorial we decided to keep it simple). 

```python
chapVerse = input(
        f"Please enter the Chapter and verse you'd like to select from {index_title},separated by a colon [ex: '12:1', '1:4']. (:) ")

ref = f"{index_title} {chapVerse}"
```

Next, we'll query the [Texts (v3)](ref:get_api-v3-texts-tref) API to retrieve the text of the given verse in English to display for the user. Hopefully this will make it easier for them to guess! 

```python
url = f"https://www.sefaria.org/api/v3/texts/{ref}?version=english"
response = requests.get(url)
texts = response.json()

print(f"Here's the text of the verse you selected:{texts['versions'][0]['text']}")
```

Now that the user sees the verse in front of them, let's ask them to guess some topics possibly related to this verse.

First, we'll query for the related topics:

```python
url = f"https://www.sefaria.org/api/ref-topic-links/{ref}"
response = requests.get(url)
topics = response.json()
```

Let's generate a set (so every entry is unique) of the topics we get back, since we don't need all of the other metadata. We'll also strip out the hyphens between words of multi-word topics so it's easier to guess (i.e. `mount-sinai` will become `mount sinai`). 

```python
topicList = [topic["topic"].replace("-", " ") for topic in topics]
topic_set = set(topicList)
```

Great! Now that we have all of the data, let's prompt our user to guess:

```python
q3 = input("Can you guess a topic related to this text?").lower()

if q3 in topic_set:
  score += 1
  print(f"Yes! You got it right! That topic is connected to this verse! FINAL SCORE: {score}/3")

else:
  print(f"Sorry! That's incorrect. The topics related to this verse are: {topic_set}. FINAL SCORE: {score}/3")
  return
```

From the user's end, this might look like this:

```
OK let's get a little more specific now!
Please enter the Chapter and verse you'd like to select from Exodus, separated by a colon [ex: '12:1', '1:4']. (:) 
>>> 19:4

Here's the text of the verse you selected:
 ‘You have seen what I did to the Egyptians, how I bore you on eagles’ wings and brought you to Me.
 
Can you guess a topic related to this text?
 >>> Mount Sinai
Yes! You got it right! That topic is connected to this verse! FINAL SCORE: 3/3
```

<Callout icon="🎉" theme="default">
  ### Congratulations! You've officially written a basic Tanakh trivia game using the data from the Sefaria API.
</Callout>

 We invite you to dive deeper into our API, and all of the infinite possibilities with our data in the documentation. Looking forward to seeing what you can build!

# Full Code

Below we'll provide the full working code for this game. We've generalized some repeating code into rudimentary functions, and added some first steps of data validation/checking to avoid unnecessary erroring. 

```python
import requests


# Generalized function for the specific API calls in this tutorial
def sefaria_get(endpoint, index_or_ref=None, params=None):
    url = f"https://www.sefaria.org/api/{endpoint}/{index_or_ref}"
    if params:
        url = f"{url}?{params}"
    response = requests.get(url)
    if response.status_code != 200:
        print("Invalid citation. Please restart the game and try again")
        return None
    return response.json()


# Main game function
def game():
    print("Welcome to the Tanakh trivia game!!")

    score = 0
    index_dict = {"A": "Exodus", "B": "Joshua", "C": "Esther"}
    
		# Display index options
    for key in index_dict:
        print(f"Option {key}: {index_dict[key]}")
        
    # Collect user input, and capitalize
    s = input("Select a text from the list by pressing the corresponding letter key ['A', 'B', 'C']: ").upper()

    # Respond to user input
    if s in index_dict.keys():
        print(f"You picked {index_dict[s]}")
        index_title = index_dict[s]
    else:
        print("Sorry! That's not a valid option. Restart the game and try again.")
        return

    # Query the Shape API
    shape = sefaria_get("shape", index_or_ref=index_title)
    if not shape:
        return None
    
    # Save answer for question 1
    answer = shape[0]["length"]

    print("Here are some trivia questions about that text:")
    
    # Question 1
    q1 = input(f"How many chapters are in {index_title}? ")

    # Check if response is an int
    try:
        q1 = int(q1)
    except Exception as e:
        print("Sorry, that's not a number. Please restart the game and try again")
        return

    # Check answer
    if answer == q1:
        print("Yes! You got it right!")
        score += 1
    else:
        print(f"Sorry! That's incorrect. The correct answer was: {answer}. You answered {score}/3 question(s)")
        return

    # Question 2
    q2 = input(f"Which chapter of {index_title} is the shortest? ")

    # Calculate which chapter of the text is the shortest (fewest verses)
    shortest = min(shape[0]["chapters"]) + 1

    # Check if input is an int
    try:
        q2 = int(q2)
    except Exception as e:
        print("Sorry, that's not a number. Please restart the game and try again")
        return

    # Check if answer is correct
    if shortest == q2:
        print("Yes! You got it right!")
        score += 1
    else:
        print(f"Sorry! That's incorrect. The correct answer was: {shortest}. You answered {score}/3 question(s)")
        return

    print("OK let's get a little more specific now!")

    # Prompt user for specific verse reference
    chapVerse = input(
        f"Please enter the chapter and verse you'd like to select from {index_title}, separated by a colon [ex: '12:1', '1:4']. (:) ")

    # Create a text reference
    ref = f"{index_title} {chapVerse}"
    
    # Query the v3 texts API
    texts = sefaria_get("v3/texts", index_or_ref=ref, params="version=english")
    if not texts:
        return None
    print(f"Here's the text of the verse you selected:\n {texts['versions'][0]['text']}")

    # Question 3
    q3 = input("Can you guess a topic related to this text?").lower()

    # Query the ref-topic-links endpoint
    topics = sefaria_get("ref-topic-links", index_or_ref=ref)
    if not topics:
        return None
      
		# Create a set of topics, removing the hyphens and replacing with spaces to make
    # it more intuitive for users guessing
    topicList = [topic["topic"].replace("-", " ") for topic in topics]
    topic_set = set(topicList)

    # Check if user answered correctly
    if q3 in topic_set:
        score += 1
        print(f"Yes! You got it right! That topic is connected to this verse! FINAL SCORE: {score}/3")

    else:
        print(f"Sorry! That's incorrect. The topics related to this verse are: {topic_set}.FINAL SCORE: {score}/3")
        return


if __name__ == '__main__':
    game()

    
```

# Notes:

* As stated above, this tutorial is just that - a tutorial to whet your appetite and show you different ways of using our data
* Any attempt to flesh this out to something ready for "real" users would need more data validation, checks, `try` / `except` blocks and more. 
* Up for the challenge? Feel free to give it a try and build off this, and [let us know](https://developers.sefaria.org/page/contact-us)
