---
title: "First Look at Hugo"
date: 2021-06-09T09:33:02+02:00
category: notes
tags:
    - Hugo
    - static site generator
    - front matters
    - comfort zone
    - nudge
    - fastest framework
keywords:
    - Hugo
    - First post
---

## First post
Because this is my first post I want to share why I want this place and how the setup works!

## why?
### name
I found it hard to identify myself to write a blog, I decided that I'm actually writing _docs for my hobbies!_ When I think about it this way it's much easier to write!

### Comfort zone
I want to get out of my **comfort zone** and start sharing and creating! I consume a lot of content around creating things and I also want to contribute by creating this space with things I found interesting!

### Nudge myself
I really don't have the motivation to continue with projects when I'm finally done with my day and the kids are asleep! I would like to write the docs periodically and get it as a habit, I want to create! I think the need for content will **nudge** me to!

## how?
### Hugo -> Static site generator
I asked two friends with blogs what tools they used and one of my friends was using WordPress and the other friend was using **Hugo**. I already have some experience with WordPress but as a true frontend developer I of course had tunnel vision on the tool I had no experience of (**Hugo**).

### Quick start
I started by going to [their site](https://gohugo.io/) and clicked *Quick Start*

I followed the simple step by step guide!
I was really impressed by the speed it was building and serving, it feels like it's already built instantly on `hugo server -D` enter -> keypress or maybe that's how it feels like coming from the code base I spend my working days in 😂. I picked a [theme](https://themes.gohugo.io/hugo-theme-cactus/) that looked nice and I was already up and running with _my hobbby docs_, it was a *quick* and easy setup!

### How it works
I just had to understand more *how it works* and looked at a few tutorials on YouTube and then started creating my own _theme_ and I really like how the html templates and the content markdown files together create the page!

#### front matters
All the content markdown files have a yaml syntax in the top that **Hugo** call **front matters** where you specify metadata that you can use in the templates:

```md
---
title: "First Look at Hugo"
date: 2021-06-09T09:33:02+02:00
draft: true
category: notes
tags:
    - Hugo
    - static site generator
    - front matters
    - comfort zone
    - nudge
    - fastest framework
keywords:
    - Hugo
    - First post
---
```

Then the title can be used in the html files using this Go template syntax `{{ .Title }}` and notice all the metadata is capitalized. This is how the title is used in the `posts/single.html` file in the theme I'm currently using now:

```html
<h1 class="posttitle" itemprop="name headline">
{{ .Title }}
</h1>
```


#### content
Rest of the markdown files will be treated as content and generate the hmtl!

## Git and Netlify
Then I created a new repo on [my github](https://github.com/DennyJohansson) and pushed the code, went to Netlify and with help from *Hugo's* [Netflify guide](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/) it went pretty smooth!

#### git submodule issue
All themes are located in `themes/` as git submodules and I had 1 issue with Netflify, I had to add a .gitmodules file to specify the path and url! Adding a new git submodule should create this file but somehow it was missing for me!

```.gitmodules
[submodule "themes/cactus"]
	path = themes/cactus
	url = https://github.com/monkeyWzr/hugo-theme-cactus.git
```


// Denny Johansson
