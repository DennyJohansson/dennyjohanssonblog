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

## The sourdough obsession
### It all started on Ornö
My family celibrated new years eve 2019 at our friends parents house in Stockholm Archipelago! It was a really great, we cooked a lot of good food and slept early because our daugther was 2 and our friends was pregnant! Anywaaay I found this dough machine and tried to bake some bread, I totally got obsessed! The next months I was baking bread daily, and shortly I started my own sourdough and also joined a sourdough forum on Facebook! Now 2,5 years later I still have the same sourdough but I only bake once a week! I really love working with a dough, it makes me happy!

### Bakers percentage and the road the perfect bread
I always use the bakers percentage when I start my dough! I'm constantly testing different percentages to one day get the perfect bread.

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

I been a fan of Clojure since the first time I saw it, it's an functional language on the Java platform but as a Javascript dev I'm mostly exited to write ClojureScript! In ClojureScript you can write Clojure but can still import Javascript libraries with npm.

There is 3 diff extensions for Clojure

- `clj` compiles to java
- `cljs` compiles to javascript
- `cljc` can be compiled to either but then you can't use javascript

### Immutable data structures
Clojure have immutable data structures and if you want to mutate something there is a few ways to do that and one way is to use an atom. An atom is a container for a value and the value can be swaped with a new value, it makes it more explicit!

### Reagent

[Reagent](https://github.com/reagent-project/reagent) is a React wrapper for ClojureScript! I really wanted to try this because it looks so clean!

I reused these examples from [Reagents git](https://github.com/reagent-project/reagent#examples)!

```example from Reagents git
(defn some-component []
  [:div
   [:h3 "I am a component!"]
   [:p.someclass
    "I have " [:strong "bold"]
    [:span {:style {:color "red"}} " and red"]
    " text."]])
```

#### r/atom
Reagent have their own version of atom that will re-render components when needed! It's like Redux but without any boilerplate! ClojureScript already have immutable data structures and atoms so it's just to create an atom, deref it or swap it!

```Also example from Reagents git
(defonce click-count (r/atom 0))

(defn state-ful-with-atom []
  [:div {:on-click #(swap! click-count inc)}
   "I have been clicked " @click-count " times."])

```

### REPL, Read Eval Print Loop 
Also worth mentioning is the Clojure REPL! You can run evaluate the code inside the editor like it was the browser console, also a REPL is connected to the browser. I have not been experimenting anything with it and can only imagine what to do with it!

### Performance of Reagent
[Reagent mentions](https://github.com/reagent-project/reagent#performance) that sometimes Reagent is actually faster then React a lot of times!

### The work Dough calculator
I started creating this first project a long time ago but then I got so frustrated with the range fields logic and never really got it to work! I picked it up again and restarted it and this time my brain had already figured out how it should be done and it went pretty fast!

To start a Reagent project I first needed to install [leiningen](https://leiningen.org/) and OpenJDK v8, I used brew for both:

```
brew install leiningen
brew install openjdk@8
```

Leiningen is the npm/yarn of Clojure and can be used to start up a new Reagent project similar to `Create React App`

```
lein new reagent doughcalc +figwheel
```

This will create the new Reagent project and also add [figwheel](https://figwheel.org/), I will use figwheel to build and hot load. Figwheel also connects a REPL that can be used! I saw a great [youtube talk](https://www.youtube.com/watch?v=j-kj2qwJa_E) from the Author of figwheel, it's acutally a fantastic talk that I highly recommend! Anyway fighweel is now added as a task on leiningen:

```
lein fighweel
```

Now the dev environment is up and running and ready for code! I browsed around and found an example very close to what I needed! It was an 3 range fields calculating BMI, this was a good starting point for me being new to this language and libraries. The code I now had was 3 parts:

1. my atom state using the reagent atom containing the initial state, basically functioning like Redux.
```
(def perc-data (reagent/atom (calc-perc {:flour 1600 :liquid 1200 :sourdough 20 })))
```

2. The `calc-perc` function is calculating the bakers percentage! 
```
(defn calc-perc [{:keys [flour liquid perc] :as data}]
  (let [h (/ flour 100)]
    (if (nil? perc)
      (assoc data :perc (/ liquid h)) 
      (assoc data :liquid (* perc h)))))
```
3. The range slider component that is being used to choose flour, liquid, percentage and sourdough
```
(defn slider [param value min max invalidates]
  [:input {:type "range" :value value :min min :max max
           :style {:width "100%"}
           :on-change (fn [e]
	                (let [new-value (js/parseInt (.. e -target -value))]
			  (swap! perc-data
			         (fn [data]
				   (-> data
			             (assoc param new-value)
				     (dissoc invalidates)
				     calc-perc)))))}])
```
4. Then we have the whole page with header and the sliders
```
(defn perc-component []
  (let [{:keys [liquid flour perc sourdough]} @perc-data
        [color diagnose] (cond
	                 (< perc 50) ["orange" "dry"]
			 (< perc 85) ["inherit" "normal"]
			 (< perc 100) ["orange" "wet"]
		         :else ["red" "soup"])]
    [:div
      [:h3 "Dough percentage calculator"]
      [:div 
       "flour: " (int flour ) "g"
       [slider :flour flour 100 2000 :perc]]
      [:div
       "liquid: " (int liquid ) "g"
       [slider :liquid liquid 30 2000 :perc]]
      [:div
       "perc: " (int perc) "% "
       [:span {:style {:color color}} diagnose]
       [slider :perc perc 10 120 :liquid ]]
      [:div
       "sourdough " (int sourdough) "%: " (* sourdough (/ flour 100)) "g"
       [slider :sourdough sourdough 10 40 :perc]]
      [:div
       "salt 2%: " (* 2 (/ flour 100)) "g"]]))
```

### MVP done, v1.0 tag
This is basically it for the MVP of [this project](https://github.com/DennyJohansson/baking)! I have created a git tag v1.0 and it will be in the state as it is now when I'm writing this!


#### Continue???
If I continue with this there are a few things I wanted to do:
- [ ] I want to create some smooth slider to easily inc/dec the numbers! Then I would't need to add a max limit!
- [ ] Save the state in localStorage
- [ ] Being able to save history to server, perhaps using [re-frame](https://github.com/day8/re-frame)
 
TODO: add bread and project images + link to prject demo 

