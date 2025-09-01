---
title: Topic Ontology
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: api-wiki
      title: The Sefaria API
---
To learn more about topics, see the [Topics API](ref:get_api-topics-topic-slug). 

Sefaria's Topics collection is organized as an ontology based on [BFO](https://en.wikipedia.org/wiki/Basic_Formal_Ontology). Below is a basic overview of BFO's structure, followed by some of the more important nodes Sefaria has added. The `slug` is the unique ID used in our database.

# BFO Overview

BFO stands for Basic Formal Ontology, and it is a foundational ontology that provides a set of basic terms and relationships to describe the structure of reality. BFO aims to provide a common framework for describing entities and their relationships in a way that is independent of any specific domain. Below, we will explain how Sefaria builds on this to create our own topic ontology.

For a complete documentation of BFO, see [here](https://github.com/BFO-ontology/BFO#implementations).

Below are some notes that should help give an idea of what each node means.

| `slug`                                      | Name                                      | Description                                                                                                                                                                                                                                                                                         | Parents                             |
| ------------------------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `entity`                                    | Entity                                    | Root node                                                                                                                                                                                                                                                                                           | N/A                                 |
| `continuant`                                | Continuant                                | Can only be sliced in space and not in time. Entity that continues to exist through time while maintaining it’s identity                                                                                                                                                                            | `entity`                            |
| `occurent`                                  | Occurent                                  | Where continuants get applied / executed. Can be sliced in time.                                                                                                                                                                                                                                    | `entity`                            |
| `independent-continuant`                    | Independent Continuant                    | Exists independent of anything else. Ex: People, planets, ping pong balls.                                                                                                                                                                                                                          | `continuant`                        |
| `specifically-dependent-continuant`         | Specifically Dependent Continuant         | Ex: Qualities. Requires some other instance to exist for it to exist. Can’t have headache without head. Can’t have color without surface                                                                                                                                                            | `continuant`                        |
| `generically-dependent-continuant`          | Generically Dependent Continuant          | Ex: PDF file or a law. Not dependent on a single entity to exist but dependent in general on another entity                                                                                                                                                                                         | `continuant`                        |
| `acts`                                      | Process                                   | Always is 1D. An act which takes time to complete                                                                                                                                                                                                                                                   | `occurent`                          |
| `process-boundary`                          | Process Boundary                          | Beginning of process, end, threshold in process. By definition occupies 0d temporal region                                                                                                                                                                                                          | `occurent`                          |
| `time`                                      | Time                                      | A temporal region is an occurrent entity that is part of time as defined relative to some reference frame.                                                                                                                                                                                          | `occurent`                          |
| `spatiotemporal-region`                     | Spatiotemporal Region                     | A spatiotemporal region is an occurrent entity that is part of spacetime.                                                                                                                                                                                                                           | `occurent`                          |
| `material-entity`                           | Material Entity                           | Rough Definition: Everything that has molecules as parts                                                                                                                                                                                                                                            | `independent-continuant`            |
| `immaterial-entity`                         | Immaterial Entity                         |                                                                                                                                                                                                                                                                                                     | `independent-continuant`            |
| `quality`                                   | Quality                                   | The ways something is. E.g. width, color.                                                                                                                                                                                                                                                           | `specifically-dependent-continuant` |
| `realizable-entity`                         | Realizable Entity                         | An entity which, when a certain action is performed, is realized.                                                                                                                                                                                                                                   | `specifically-dependent-continuant` |
| `process-profile`                           | Process Profile                           |                                                                                                                                                                                                                                                                                                     | `acts`                              |
| `instant`                                   | Zero-Dimensional Temporal Region          |                                                                                                                                                                                                                                                                                                     | `time`                              |
| `one-dimensional-temporal-region`           | One-Dimensional Temporal Region           |                                                                                                                                                                                                                                                                                                     | `time`                              |
| `object`                                    | Object                                    | Ex. human (boundary is skin). Entity with continuous boundary.                                                                                                                                                                                                                                      | `material-entity`                   |
| `fiat-object-part`                          | Fiat Object Part                          | Ex. arm, wall of room, tabletop. Subjective part of object that people refer to. arm doesn’t have complete boundary.                                                                                                                                                                                | `material-entity`                   |
| `object-aggregate`                          | Object Aggregate                          | Ex. population, nation. Only contains members which are themselves objects or object aggregates.                                                                                                                                                                                                    | `material-entity`                   |
| `relational-quality`                        | Relational Quality                        |                                                                                                                                                                                                                                                                                                     | `quality`                           |
| `role`                                      | Role                                      |                                                                                                                                                                                                                                                                                                     | `realizable-entity`                 |
| `disposition`                               | Disposition                               |                                                                                                                                                                                                                                                                                                     | `realizable-entity`                 |
| `places`                                    | Site                                      | Ex: Valleys, tunnels, dents, digestive tract, airspace. Covers various types of holes. 3D, not made of matter and bounded partly  by material entity or  all fiat boundaries. Manhattan is a site when referring to the space above manhattan island where people live and not the physical island. | `immaterial-entity`                 |
| `continuant-fiat-boundary`                  | Continuant Fiat Boundary                  | Ex: Entrance to a cave. Doesn’t have to be completely mathematical. A way of demarcating objects.                                                                                                                                                                                                   | `immaterial-entity`                 |
| `zero-dimensional-continuant-fiat-boundary` | Zero-dimensional Continuant Fiat Boundary |                                                                                                                                                                                                                                                                                                     | `continuant-fiat-boundary`          |
| `one-dimensional-continuant-fiat-boundary`  | One-dimensional Continuant Fiat Boundary  |                                                                                                                                                                                                                                                                                                     | `continuant-fiat-boundary`          |
| `two-dimensional-continuant-fiat-boundary`  | Two-dimensional Continuant Fiat Boundary  |                                                                                                                                                                                                                                                                                                     | `continuant-fiat-boundary`          |
| `function`                                  | Function                                  |                                                                                                                                                                                                                                                                                                     | `disposition`                       |

# Overview of Sefaria Nodes

Below are some of the most important nodes Sefaria added to the ontology. Many are self-explanatory. They are broken up by which BFO node they're under.

## Process (aka Acts)

| `slug`                | Name                | Parents   |
| --------------------- | ------------------- | --------- |
| `musical-process`     | Musical Process     | `art`     |
| `history`             | History             | `acts`    |
| `lifecycle-events`    | Lifecycle Events    | `acts`    |
| `halachic-process`    | Halachic Process    | `acts`    |
| `art`                 | Art                 | `acts`    |
| `weather`             | Weather             | `acts`    |
| `theological-process` | Theological Process | `acts`    |
| `biblical-event`      | Biblical Event      | `history` |

## Temporal Region

| `slug`              | Name              | Parents                           |
| ------------------- | ----------------- | --------------------------------- |
| `weeks`             | Weeks             | `one-dimensional-temporal-region` |
| `months`            | Months            | `one-dimensional-temporal-region` |
| `seasons`           | Seasons           | `one-dimensional-temporal-region` |
| `years`             | Years             | `one-dimensional-temporal-region` |
| `part-of-day`       | Part of Day       | `one-dimensional-temporal-region` |
| `days`              | Days              | `one-dimensional-temporal-region` |
| `fasting`           | Fasting           | `days`                            |
| `holidays`          | Holidays          | `one-dimensional-temporal-region` |
| `rabbinic-holidays` | Rabbanic Holidays | `holidays`                        |
| `torah-holidays`    | Torah Holidays    | `holidays`                        |
| `shalosh-regalim`   | Shalosh Regalim   | `torah-holidays`                  |
| `high-holidays`     | High Holidays     | `torah-holidays`                  |

## Realizable Entity

| `slug`                              | Name                              | Parents                           |
| ----------------------------------- | --------------------------------- | --------------------------------- |
| `freedom`                           | Freedom                           | `disposition`                     |
| `middot`                            | Middot                            | `disposition`                     |
| `emotions`                          | Emotions                          | `disposition`                     |
| `knowledge`                         | Knowledge                         | `disposition`                     |
| `divine-names`                      | Divine Names                      | `role`                            |
| `halachic-role`                     | Halachic Role                     | `role`                            |
| `role-of-person`                    | Role of Person                    | `role`                            |
| `halachic-role-of-inanimate-object` | Halachic Role of Inanimate Object | `halachic-role`                   |
| `halachic-role-of-person`           | Halachic Role of Person           | `halachic-role`, `role-of-person` |
| `family-roles`                      | Family Roles                      | `role-of-person`                  |
| `vocations`                         | Vocations                         | `role-of-person`                  |

## Qualities

| `slug`                                  | Name                                  | Parents                        |
| --------------------------------------- | ------------------------------------- | ------------------------------ |
| `halachic-quality`                      | Halachic Quality                      | `quality`                      |
| `material-quality`                      | Material Quality                      | `quality`                      |
| `numbers`                               | Numbers                               | `quality`                      |
| `relational-quality`                    | Relational Quality                    | `quality`                      |
| `colors`                                | Colors                                | `material-quality`             |
| `preposition`                           | Preposition                           | `relational-quality`           |
| `covenants`                             | Covenants                             | `relational-quality`           |
| `specific-person-relationship`          | Specific Person Relationship          | `relational-quality`           |
| `specific-biblical-person-relationship` | Specific Biblical Person Relationship | `specific-person-relationship` |

## Generically Dependent Continuants

The topic `religion` here should likely be called `religious-idea` or something similar.

| `slug`                | Name                | Parents                            |
| --------------------- | ------------------- | ---------------------------------- |
| `laws`                | Laws                | `generically-dependent-continuant` |
| `philosophy`          | Philosophy          | `generically-dependent-continuant` |
| `science`             | Science             | `generically-dependent-continuant` |
| `language-entity`     | Language Entity     | `generically-dependent-continuant` |
| `texts`               | Texts               | `generically-dependent-continuant` |
| `ethics`              | Ethics              | `generically-dependent-continuant` |
| `halakhah`            | Halakhah            | `law`                              |
| `halachic-principles` | Halachic Principles | `halakhah`                         |
| `language`            | Language            | `language-entity`                  |
| `letters`             | Letters             | `language-entity`                  |
| `phrase`              | Phrase              | `language-entity`                  |
| `words`               | Words               | `language-entity`                  |
| `religion`            | Religion            | `philosophy`                       |
| `magic`               | Magic               | `philosophy`                       |
| `spiritual-places`    | Spiritual Places    | `religion`                         |
| `theological-tenets`  | Theological Tenets  | `religion`                         |
| `biblical-source`     | Biblical Source     | `texts`                            |

## Object Aggregate

| `slug`                       | Name                       | Parents                     |
| ---------------------------- | -------------------------- | --------------------------- |
| `group-of-living-creatures`  | Group of Living Creatures  | `object-aggregate`          |
| `group-of-inanimate-objects` | Group of Inanimate Objects | `object-aggregate`          |
| `group-of-animals`           | Group of Animals           | `group-of-living-creatures` |
| `group-of-plants`            | Group of Plants            | `group-of-living-creatures` |
| `group-of-people`            | Group of People            | `group-of-living-creatures` |
| `peoples`                    | Peoples                    | `group-of-people`           |

## Object

| `slug`             | Name             | Parents            |
| ------------------ | ---------------- | ------------------ |
| `living-creature`  | Living Creature  | `object`           |
| `inanimate-object` | Inanimate Object | `object`           |
| `food`             | Food             | `inanimate-object` |
| `animals`          | Animal           | `living-creature`  |
| `people`           | People           | `living-creature`  |
| `plants`           | Plants           | `living-creature`  |
| `biblical-figures` | Biblical Figures | `People`           |
| `mishnaic-people`  | Mishnaic People  | `People`           |
| `talmudic-people`  | Talmudic People  | `People`           |
| `geon-person`      | Geon Person      | `People`           |
| `rishon-person`    | Rishon Person    | `People`           |
| `achron-person`    | Achron Person    | `People`           |
| `modern-person`    | Modern Person    | `People`           |
| `mammals`          | Mammals          | `animals`          |
| `fish`             | Mammals          | `animals`          |
| `birds`            | Birds            | `animals`          |
| `bugs`             | Bugs             | `animals`          |
| `reptiles`         | Reptiles         | `animals`          |
| `amphibians`       | Amphibians       | `animals`          |
| `worms`            | Worms            | `animals`          |

## Immaterial Entity

| `slug`                | Name                | Parents  |
| --------------------- | ------------------- | -------- |
| `geography`           | Geography           | `places` |
| `geographic-features` | Geographic Features | `places` |
| `structures`          | Structures          | `places` |

## Fiat Object Part

| `slug`       | Name       | Parents            |
| ------------ | ---------- | ------------------ |
| `body-parts` | Body Parts | `fiat-object-part` |

# Overview of Sefaria link types

Below is an exhaustive list of all of the link types that can exist between nodes. Each topic link in our database must be one of these types. Each link is bidirectional. If reading the link in the 'forward' direction, you should use `slug`. Reading it in the backwards direction, use `inverseSlug`. For example, see this link:

```
{
    "fromTopic": "isaac",
    "toTopic": "abraham",
    "linkType": "child-of"
}
```

In the forward direction, it would be read as `isaac` -`child-of`-> `abraham`. In the backwards direction, it would be `abraham`-`parent-of`-> `isaac`. All links in our database are saved using the link type's `slug`, not `inverseSlug`. 

Links with the same `slug` and `inverseSlug` are symmetrical. Currently, there has been no work done to infer topic links from existing links (e.g. if we know "a" is a child of "b", and "c" is a sibling of "a", then "c" is a child of "b").

## Link Types Between People

The following are link types that are meant to connect two different people

| `slug`              | `inverseSlug`       | Description                                                                                  |
| ------------------- | ------------------- | -------------------------------------------------------------------------------------------- |
| `child-of`          | `parent-of`         |                                                                                              |
| `child-in-law-of`   | `parent-in-law-of`  |                                                                                              |
| `sibling-of`        | `sibling-of`        |                                                                                              |
| `descendant-of`     | `ancestor-of`       | Used for relating a famous descendant to their ancestor. e.g. David -`descendant-of` -> Ruth |
| `taught`            | `learned-from`      | Usually used for relating a rabbi and his student                                            |
| `corresponded-with` | `corresponded-with` | Two people who spoke with each other (and do not fit under another link type)                |
| `opposed`           | `opposed`           | Two people who were opposed to each other in their views                                     |
| `cousin-of`         | `cousin-of`         |                                                                                              |
| `spouse-of`         | `spouse-of`         |                                                                                              |

## Link Types Between Other Types of Nodes

Each link type is only valid if the `fromTopic` is of type `Valid From` and if the `toTopic` is of type `Valid To`.

| `slug`                         | `inverseSlug`                  | Description                                                                                                                                                                                                             | Valid From                                                                | Valid To                 |
| ------------------------------ | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------ |
| `has-role`                     | `role-of`                      | physical entity which has a social role. e.g. David -`has-role`-> king                                                                                                                                                  | `material-entity`                                                         | `role`                   |
| `gender-of`                    | `has-gender`                   | Gender of a person. e.g. male -`gender-of`-> david                                                                                                                                                                      | `gender`                                                                  | `people`                 |
| `person-participates-in-event` | `event-has-person-participant` | A person who participates in an historical event                                                                                                                                                                        | `people`                                                                  | `history`                |
| `adjacent-to`                  | `adjacent-to`                  | An independent thing which is next to another thing. e.g. menorah -`adjacent-to`-> shulchan                                                                                                                             | `independent-continuant`                                                  | `independent-continuant` |
| `causes`                       | `has-cause`                    | An action which causes another action                                                                                                                                                                                   | `acts`                                                                    | `acts`                   |
| `member-of`                    | `has-member`                   | A physical thing which is part of a group. e.g. Judah Maccabee -`member-of`-> Maccabees                                                                                                                                 | `material-entity`                                                         | `object-aggregate`       |
| `physically-contains`          | `physically-contained-in`      | An independent thing which physically contains another independent thing. e.g. Israel -`physically-contains`-> Jerusalem                                                                                                | `independent-continuant`                                                  | `independent-continuant` |
| `temporally-contains`          | `temporally-contained-in`      | A time thing which contains another time thing. e.g. A week -`temporally-contains`-> Shabbat                                                                                                                            | `occurent`                                                                | `occurent`               |
| `participates-in`              | `has-participant`              | A non-time entity which takes part in a time entity. e.g. shabbat-candles - `participates-in`-> shabbat                                                                                                                 | `continuant`                                                              | `occurent`               |
| `precedes`                     | `preceded-by`                  | A time entity which comes before another time thing. e.g. shabbat -`precedes`-> sunday                                                                                                                                  | `occurent`                                                                | `occurent`               |
| `applies-halacha`              | `has-halachic-application`     | A halachic action which applies a certain halacha. e.g. shaking-lulav -`applies-halacha`-> laws-of-lulav                                                                                                                | `halachic-process`                                                        | `halakhah`               |
| `specifically-dependent-on`    | `has-specific-dependence`      | A specifically dependent continuant which depends on entity to exist. you cannot imagine this SDC existing without this entity. e.g. the role `god-of-the-patriarchs` is `specifically-dependent-on`-> `the-patriarchs` | `specifically-dependent-continuant` OR `generically-dependent-continuant` | `entity`                 |
| `relationship-of`              | `has-relationship`             | A relationship's connection to people in the relationship. e.g. `moshe-and-aharons-relationship` is -`specifically-dependent-on`-> moshe                                                                                | `specific-person-relationship`                                            | `people`                 |
| `leader-of`                    | `has-leader`                   | A person who was a leader of this people                                                                                                                                                                                | `people`                                                                  | `peoples`                |

## General Link Types

The following link types can connect any two nodes:

| `slug`              | `inverseSlug`           | Description                                                                                                           |
| ------------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `is-a`              | `is-category-of`        | Describes ontological hierarchy between nodes. A - `is-a` -> B implies that B is a more general type of A             |
| `displays-under`    | `displays-above`        | Describes display structure of nodes. This is used to display the table of contents for topics on Sefaria's frontend. |
| `synonymous-with`   | `synonymous-with`       | Two topics which are almost always interchangeable. e.g. `totafot` is -`synonymous-with`-> teffilin                   |
| `similar-to`        | `similar-to`            | Two topics which are similar in many of their attributes. e.g. `saving` is -`similar-to`-> `rescuing`                 |
| `dissimilar-to`     | `dissimilar-to`         | Two topics which are not similar in many of their attributes. e.g. `work` is -`dissimilar-to`-> `rest`                |
| `related-to`        | `related-to`            | Two topics which are generally related. Usually these two topics' relationship doesn't fit under any other link type  |
| `sheets-related-to` | `has-sheets-related-to` | Two topics whose relationship was derived from colocation of tags on source sheets                                    |