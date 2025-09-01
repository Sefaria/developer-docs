---
title: Data Visualization
excerpt: A basic tutorial on using the Sefaria API to create data visualizations.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Topics Treemap

In this tutorial, we're going to using the [Topics API](https://developers.sefaria.org/reference/get-all-topics) to build a basic data visualization using [D3Plus](https://d3plus.org/?path=/docs/introduction--d3plus). 

You can see the entire repository [here](https://github.com/Sefaria/dataviz-tutorial). 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9336436205922c01d519d8a7bdb15182a5721f407e1c19ed0c14192e6eade1a9-Screenshot_2025-03-06_at_11.30.03.png",
        "",
        "Visualization Screenshot"
      ],
      "align": "center"
    }
  ]
}
[/block]


## Tutorial Set Up

For this tutorial, we'll be using [React](https://react.dev/).

### Creating the App

Make sure you have [Node](https://nodejs.org/en) installed. We'll be using [Vite](https://vite.dev/) to create the basic template for the React app. 

1. Run `npx create-vite@latest <name-of-app> --template react`
2. Then, `cd <name-of-app>` 
3. Install your dependencies via `npm install`
4. Run the app via `npm run dev`

You should see something running at http\://localhost:5173/. 

### Install D3Plus for React

We used the [D3Plus](https://d3plus.org/?path=/docs/introduction--d3plus) library to generate this visualization. 

Run `npm install d3plus-text` and `npm install d3plus-react` to get the required D3Plus packages. 

## App.jsx

This tutorial will focus only on the code we wrote in `App.jsx`, the main body of the application. We did make some other tweaks for styling and streamlining as well, so if you'd like to see those, see the full GitHub repository containing this project [here](https://github.com/Sefaria/dataviz-tutorial). The [README](https://github.com/Sefaria/dataviz-tutorial/blob/main/README.md) has notes for running it locally as well. 

We're going to go through the file line-by-line, to see everything together - check out the [repo](https://github.com/Sefaria/dataviz-tutorial) or scroll to the bottom. 

# Building the Visualization

Since Vite gives you templated content for `App.jsx`, your first step is to delete the entire contents of the file. We'll be replacing it with the code below. 

## Imports

At the top of your file, add the necessary imports. We need the [useEffect](https://react.dev/reference/react/useEffect) and [useState](https://react.dev/reference/react/useState), two [React hooks](https://react.dev/reference/react/hooks), as well as the [Treemap](https://d3plus.org/?path=/docs/charts-treemap--d3plus) visualization from D3Plus. 

```javascript
import { useEffect, useState } from "react";
import {Treemap} from "d3plus-react";
```

> 🚧 Component Structure
> 
> For the sake of simplicity and for the flow of a tutorial (where text is interwoven with code) we chose to keep everything in one big component. However, ideally it would be better if each aspect of the page was its own component rendered inside of the `<App />` component.

## useState() Hooks

We're going to assume basic knowledge of React and React hooks (and if you're new, feel free to check out the documentation linked above to learn more). 

Let's set up the top of our component with our state hooks. 

```javascript
const [data, setData] = useState([]);
const [limit, setLimit] = useState(100);
const [includePeopleTopics, setIncludePeopleTopics] = useState(false);
```

We need to track three things:

1. The `data` from the API
2. The `limit` - a query parameter for the API, how many topics do we want returned from the query
3. A boolean `includePeopleTopics` - since some topics with a subclass of `person` have an overwhelming amount of sources linked to them, we give users the ability to toggle between displaying those topics or not as part of the treemap. 

## API Call and Data Processing

### Topics API

First, let's take a look at what the [Topics API](https://developers.sefaria.org/reference/get-all-topics) returns at a high level - a list of [topics from the Sefaria library](https://www.sefaria.org.il/topics). Below, we'll zoom in on a topic, to get a sense of what data we have to work with.  

```json
[
  {
    "slug": "rabbi-yochanan-b-napacha",
    "titles": [...], // Commented out for brevity
    "subclass": "person",
    "alt_ids": {...}, // Commented out for brevity
    "properties": {...}, // Commented out for brevity
    "description": {
      "en": "R. Yochanan studied under R. Yehudah haNasi and emerged as the next leader after his death. He directed the academy in Tiberias and set up the groundwork for the Jerusalem Talmud. His unusual beauty, sharpness and personal sufferings made him a captivating yet intimidating teacher. It was due to his personality and his unrivaled learning that the center of Torah study did not move to Babylonia during his lifetime.",
      "he": "רבי יוחנן למד עם רבי יהודה הנשיא והיה למנהיג הבא אחרי מותו. עמד בראש הסנהדרין בטבריה והכין את היסודות לתלמוד הירושלמי. יופיו החריג, חריפותו וסבלו האישי עשו אותו למורה שובה אך מאיים. בשל אישיותו ותלמודו המעולה, לא עבר מרכז לימוד התורה לבבל במשך חייו."
    },
    "categoryDescription": {},
    "numSources": 24590,
    "description_published": true,
    "primaryTitle": {
      "en": "Rabbi Yochanan b. Napacha",
      "he": "רבי יוחנן"
    }
  }
]
```

For this use case, what's most important to us is the primary English title of the topic, the `numSources` field (i.e. the number of Sefaria sources linked to the topic) and `subclass` - since we want to give users the option to view non-people topics only. 

Now that we see what we're working with, let's get back to the code...

### UseEffect

Heading back to the code, inside of our `useEffect` hook, we make a call to the [Topics API](https://developers.sefaria.org/reference/get-all-topics). 

If `includePeopleTopics` is `false`, we filter out any topic with a subclass of `person`, otherwise we keep the people topics in the returned data.

Then, we use a `map` function to create an array of objects, with the field `title` and `numSources`. 

Finally, we use our `setData` hook to set this filtered, mapped data in our `data` state. 

```javascript
  useEffect(() => {
    fetch(`https://www.sefaria.org/api/topics?limit=${limit}`)
      .then((res) => res.json())
      .then((res) => {
        if (res) {
          const filteredData = res
          .filter((topic) => includePeopleTopics || !(topic.subclass && topic.subclass === "person"))
          .map((topic) => ({
            title: topic.primaryTitle?.en, 
            numSources: topic.numSources
          }));
  
          setData(filteredData);
        }
      })
      .catch((err) => console.error(err));
  }, [limit, includePeopleTopics]);
```

This `useEffect` is set to re-render any time there's a change in `limit` or `includePeopleTopics`, two values we allow the users to toggle in the controls. 

## Returning JSX

Now, we return our JSX! First, we set up some controls for the user, and then we render the visualization. Again, in an ideal world these would be broken into sub-components, but for the sake of the tutorial we kept it all together. 

### The User Controls

Inside our user controls, we have a range toggle that allows users to set the `limit` on the number of topics they'd like returned from the API. On change, we call our `setLimit` hook and change the limit to the value targeted by the user. (Recall, any change to `limit` triggers `useEffect` to run, querying the API again and refreshing the data. 

```javascript React
<h1>Sefaria Topics Data-Viz</h1>
	<div className="controls">
		<h4 className="centered">Controls</h4>
			<div>
				<span>Adjust the number of topics queried from the API: </span>
					<input
              type="range"
              min="100"
              max="5000"
              value={limit}
              step="100"
              onChange={(e) => setLimit(Number(e.target.value))}
            />
         <span>{limit}</span>
        </div>
```

Then, we also create a checkbox to toggle whether or not the user would like the "people" topics to be included or not in the display:

```Text React
        <div>
          <label>
            <input
              type="checkbox"
              checked={includePeopleTopics}
              onChange={() => setIncludePeopleTopics(!includePeopleTopics)}
             />
            Include People Topics
           </label>
        </div>   
```

On any check (or un-checking) the callback function calls our `setIncludePeopleTopics` hook to `true` or `false`.  (Recall, any change to `limit` triggers `useEffect` to run, refreshing the data by re-running it through the conditional filtering).

### The Visualization

Beneath the controls, we render the visualization. The nice thing about D3Plus is that instead of diving into the nitty gritty of D3 configuration, we get an out-of-the-box React component. All we need to do is pass in our data, and set the config the way we'd like it. Let's take a look:

```Text React
<Treemap config={{
			data: data,
      groupBy: "title",
      sum: "numSources", 
      height: 400,
      width: 1200,
      shapeConfig: {
      	label: d => {
                return [d.title];
              },
       },
       tooltipConfig: {
              tbody: (d) => [
                ["Linked Sources:", d.numSources]
              ]
      },
}} />
```

The `<Treemap/>` component takes one prop, `config`, where we can pass the data and set the configurations. You'll see we pass in the data, ask to group the topics by their `title`, and create the area value on the map based on the `sum` of the `numSources`. 

The next few lines arrange the desired height and width of the visualization, as well as overriding some defaults to make things clean and presentable. (`shapeConfig` allows us to hide percentage values, which make less sense in this case... and `tooltipConfig` does the same, just in the context of the hover-able tooltip).

> 🎉 Congratulations!
> 
> You've officially built your first data visualization using data from the Sefaria API!

# Further Expansion

Obviously, this example is extremely bare-bones and just meant as a proof of concept. Can you take this example and expand? 

Some possibilities:

- Experimenting with the use of different visualizations
- Experimenting with a different visualization library
- Experimenting with different API endpoints, what's most the most useful visualization you can make based on the Jewish canon?

If you build something, [let us know](https://developers.sefaria.org/page/contact-us)! We love seeing all of the projects [powered by our data](https://developers.sefaria.org/docs/powered-by-sefaria)!

***

## Full Code

To see the full repository, including style changes and other clean up, see [here](https://github.com/Sefaria/dataviz-tutorial). 

To see the entirety of the `App.jsx` file we built, see below:

```javascript
import { useEffect, useState } from "react";
import {Treemap} from "d3plus-react";

function App() {
  const [data, setData] = useState([]);
  const [limit, setLimit] = useState(100);
  const [includePeopleTopics, setIncludePeopleTopics] = useState(false);

  useEffect(() => {
    fetch(`https://www.sefaria.org/api/topics?limit=${limit}`)
      .then((res) => res.json())
      .then((res) => {
        if (res) {
          const filteredData = res
          .filter((topic) => includePeopleTopics || !(topic.subclass && topic.subclass === "person"))
          .map((topic) => ({
            title: topic.primaryTitle?.en, 
            numSources: topic.numSources
          }));
  
          setData(filteredData);
        }
      })
      .catch((err) => console.error(err));
  }, [limit, includePeopleTopics]);

  return (
    <>
        <h1>Sefaria Topics Data-Viz</h1>
        <div className="controls">
          <h4 className="centered">Controls</h4>
          <div>
            <span>Adjust the number of topics queried from the API: </span>
            <input
              type="range"
              min="100"
              max="5000"
              value={limit}
              step="100"
              onChange={(e) => setLimit(Number(e.target.value))}
            />
            <span>{limit}</span>
          </div>
          <div>
            <label>
              <input
                type="checkbox"
                checked={includePeopleTopics}
                onChange={() => setIncludePeopleTopics(!includePeopleTopics)}
              />
              Include People Topics
            </label>
          </div>   
        </div>

        <div className="viz">
          {data.length > 0 ? (
          <Treemap config={{
            data: data,
            groupBy: "title",
            sum: "numSources", 
            height: 400,
            width: 1200,
            shapeConfig: {
              label: d => {
                return [d.title];
              },
            },
            tooltipConfig: {
              tbody: (d) => [
                ["Linked Sources:", d.numSources]
              ]
            },
          }} />
          ) : (
            <p>Loading...</p>
          )}
        </div>
      </>
  );
}

export default App;

```