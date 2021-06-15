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
We celibrated new years eve 2019 at our friends parents house in Stockholm archipelago! It was really chill, we cooked good food, played cards and slept early because our daugther was 2 years old and our friends was pregnant! Anywaaay I found this __dough__ machine and tried to bake some bread, I totally got obsessed! The next months I was baking bread daily, and shortly I started my own __sourdough__ and also joined a forum on Facebook! Now 2,5 years later I still have the same __sourdough__ but I only bake once a week! I really love working with a __dough__, it makes me happy!

![bread image](/img/sourdough-breads.jpg)

### Bakers percentage
I always use the bakers percentage when I start my dough, to calculate the bakers percentage of a dough the flour will be the base:

- 1000g flour
- 800g liquid

This would give an __bakers percentage of 80%__! Then I have around 10%-20% sourdough and 2% salt.

- 1000g flour
- 800g liquid (80%)
- 150g dourdough (15%)
- 20g salt (2%)

I always have to calculate and write this down and decided to make my life a little easier and started to work on a dough calculater app!

### Tech choice!
I wanted o try writing React with __Clojure__ and decided to go with __Reagent__!

## Clojure
Rich Hickey is the creator of Clojure and I love watching youtube videos on he's talks! He also have created an immutable database called Datomic!

I been a fan of Clojure since the first time I saw it, it's an functional language on the Java platform but as a Javascript dev I first noticed it because of ClojureScript! In ClojureScript you can write Clojure and still import Javascript libraries with npm, but it depends on the extension.

There is 3 diff extensions for Clojure

- `clj` compiles to java
- `cljs` compiles to javascript
- `cljc` can be compiled to either but then you can't use javascript

### Immutable data structures and atom
Clojure have immutable data structures and if you want to use something as a state you can use atoms, an atom is a container for a value and the value can be swaped with a new value. Atoms are free of race-conditions and works very well with mutli threads.

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
Reagent have their own version of __atom__ that will re-render components when needed, it's like Redux but without any boilerplate.

```Also example from Reagents git
(defonce click-count (r/atom 0))

(defn state-ful-with-atom []
  [:div {:on-click #(swap! click-count inc)}
   "I have been clicked " @click-count " times."])

```

### REPL, Read Eval Print Loop 
Also worth mentioning is the __Clojure REPL__! You can evaluate the code inside the editor like it was the browser console, also a REPL is connected to the browser. I have not been experimenting anything with it and can only imagine what to do with it!

### Performance of Reagent
[Reagent mentions](https://github.com/reagent-project/reagent#performance) that sometimes Reagent is actually __faster then React__ a lot of times!

## The work of Dough calculator
I started working on this project a long time ago but then I got so frustrated with the sliders logic and never really got it to work as I wanted! 

Now when I started this blog I got motivated to pick it up again, this time my brain had already figured out how it should be done and it went pretty fast!

### setup new Reagent project
To start a Reagent project I first needed to install [leiningen](https://leiningen.org/) and OpenJDK v8, I used brew for both:

```
brew install leiningen
brew install openjdk@8
```

#### Leiningen
Leiningen is the npm/yarn of Clojure and can be used to start up a new Reagent project similar to `Create React App`

```
lein new reagent doughcalc +figwheel
```

#### Figwheel
This creates the new Reagent project and also add [figwheel](https://figwheel.org/), I will use figwheel to build and hot load. Figwheel also connects a REPL that can be used! I saw a great [youtube talk](https://www.youtube.com/watch?v=j-kj2qwJa_E) from the Author of figwheel, it's acutally a fantastic talk that I highly recommend! Anyway fighweel is now added as a task on leiningen:

```
lein fighweel
```

#### Start coding
Now the project is up and running and ready to start coding! I browsed around and found an example very close to what I needed! It was 3 sliders calculating BMI, this was a good starting point for me being new to Reagent and Clojure. I modified it and it worked as expected, the code I now had was:

- My app state inside an reagent/atom state, functioning like Redux.
```
(def perc-data (reagent/atom (calc-perc {:flour 1600 :liquid 1200 :sourdough 20 })))
```

- The `calc-perc` function is calculating the bakers percentage! 
```
(defn calc-perc [{:keys [flour liquid perc] :as data}]
  (let [h (/ flour 100)]
    (if (nil? perc)
      (assoc data :perc (/ liquid h)) 
      (assoc data :liquid (* perc h)))))
```
- The slider component that is being used to choose flour, liquid, percentage and sourdough
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
- Then we have the whole calculator component dislplaying an header and the sliders
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
This is it for the MVP of [this project](https://github.com/DennyJohansson/baking)! I have created a git tag v1.0 and it will be in the state as it is now when I'm writing this!

#### Demo
I host a [Dough calc demo](https://doughcalc.herokuapp.com/) but it could look different if I have kept working on it, so I also added a screenshot: 

![doughcalc](/img/doughcalc.png)


### Continue???
When I continue with this calculator I want to do:
- [ ] Create some smooth slider to easily inc/dec the numbers! Then I would't need to add a max limit!
- [ ] Save the state in localStorage
- [ ] Being able to save history to server, perhaps using [re-frame](https://github.com/day8/re-frame)

