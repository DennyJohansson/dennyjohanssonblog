---
title: "React With Clojure"
date: 2021-06-13T20:48:02+02:00
draft: true
category: notes
tags:
    - ClojureScript
    - Reagent
    - atom
    - immutable
    - sourdough
    - bakers percentage
    - dough calculator
    - React
keywords:
    - Dough calculator
    - Reagent
    - ClojureScript
---

## Dough calculator
My family celibrated new years eve 2019 at our friends parents house in Stockholm Archipelago! It was a really great, we cooked a lot of good food and slept early because our daugther was 2 and our friends was pregnant! Anywaaay I found this dough machine and tried to bake some bread, I totally got obsessed! The next months I was baking bread daily, and shortly I started my own sourdough and also joined a sourdough forum on Facebook! Now 2,5 years later I still have the same sourdough but I only bake once a week!

### Bakers percentage
I always use the bakers percentage when I create my dough! I'm contantly testing different percentage.

To calculate the bakers percentage of a dough the flour will be the base:

- 1000g flour
- 800g liquid

This would give an bakers percentage of 80%! Then I have around 10%-20% sourdough and 2% salt.

- 1000g flour
- 800g liquid (80%)
- 150g dourdough (15%)
- 20g salt (2%)

I always have to calculate and write this down and decided to make my life a little easier and started to work on a dough calculater app!

### Tech choice
For this I wanted to try ClojureScript and Reagent!

## Clojure

I been a fan of Clojure since the first time I saw it, it's an functional language on the Java platform but as a Javascript dev I'm mostly exited to write ClojureScript! In ClojureScript you can use Clojure and Javascript and it will be compiled to Javascript!

Clojure have immutable data structures and if you want to mutate something there is a few ways to do that and one way is to use an atom. An atom is a container for a value and the value can be swaped with a new value, it makes it more explicit when an mutation is happening!

### Reagent

[Reagent](https://github.com/reagent-project/reagent) is a React wrapper for ClojureScript! I really wanted to try this because it looks so clean!

[The example Reagent](https://github.com/reagent-project/reagent#examples) is using in their git!

```
(defn some-component []
  [:div
   [:h3 "I am a component!"]
   [:p.someclass
    "I have " [:strong "bold"]
    [:span {:style {:color "red"}} " and red"]
    " text."]])
```

And also Reagent have their own version of atom that will re-render components when needed! It's like Redux but without any boilerplate! ClojureScript already have immutable data structures and atoms so it's just to create an atom, deref it or swap it!

```Also example from Reagents git
(defonce click-count (r/atom 0))

(defn state-ful-with-atom []
  [:div {:on-click #(swap! click-count inc)}
   "I have been clicked " @click-count " times."])

```

### Performance of Reagent
[Reagent mentions](https://github.com/reagent-project/reagent#performance) that sometimes Reagent is actually faster then React a lot of times!






