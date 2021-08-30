---
title: "Allfons project"
date: 2021-08-30T10:19:46+02:00
tags:
  - Typescript
  - React
  - Next.js
  - GraphQL
  - Apollo
  - PostgreSQL
  - docker
  - study
  - platform
  - design
  - Allfons
  - Azure devOps
keywords:
  - Allfons
  - React
  - Next.js
  - GraphQL
  - Typescript
---
# Our own project with our own idea 🔥
Me and some friends really wanted to start something on our own, working on something from scratch with our own idea, this was so inspiring and felt so much fun!

We had a lot of motivation and started to have brainstorm sessions to come up with any idea that sounds good enough to jump right into, we came quickly up with an idea:
**a rule book study platform for sport clubs.**

We identified the need for a platform where coaches, judges, parents and athletes could study the rule book of their sport,
so first step we wanted to **build an MVP and sell it to Hockey sport clubs!**

## Vision & planning 🔮
- We defined the project by answer some basic questions like, purpose?, description?, how to get payed? and so on.

- We defined the features we needed: 
  - [ ] browse rulebook
  - [ ] create new quiz
  - [ ] take quiz with results
  - [ ] browse quiz
  - [ ] login/sign up
  - [ ] design
  - [ ] name
- We setup the project in Azure
  - work items
  - new repo
  - build pipelines

Now when we had our work items already defined and we were ready to work!

### This is the stack we choose

- Typescript
- Next.js
- GraphQL, prisma
- PostgreSQL, docker

### Typescript
We choose to have Typescript for both client and server. I was initially very sceptical to Typescript because I have always used Javascript and think that was working great!
In the beginning I was frustrated by all Typescript errors I was writing, I had to put a lot of time to learn Typescript, when I had bumped my head into a thousand errors I started to really like it!

Starting a new feature with Typescript for me adds a planning stage where I plan the types and think about app state and names and the types can be shared from the database to the client,
this is really great and gives a good overview, the whole app is in sync by the types, Typescript gives suggestions and there is less `undefined` errors.

But, it also adds more code and slows down text editor with ts-server running!

### Next.js
Next.js enabled SSR, this gives a good instant load feeling, we were already used to React so it was a low learning curve, Next.js had a great getting started template with Typescript that we could use, it already had the folder structure in pages and routing just working!

### GraphQL
My friends were using GraphQL at their work and they liked it! This was my first experience with GraphQL and I really liked the idea of how I can **define the returned data objects** and **only get the data I need**. While learning it was great for me to have the Apollo graphQL **/playground tool**, testing around writing queries, mutations and browse the API!

## Good momentum 🚀
We had good momentum creating features, all the MVP functionality was done in a few months, all in our spare time!

Only 2 things left:

- [x] rulebook
- [x] create new quiz
- [x] take quiz with results
- [x] browse quiz
- [x] login/sign up
- [ ] design
- [ ] name

## Designer joined ✨
The design, we had a vision that we wanted micro feedback, animations and a playful design!

Luckily we got a designer friend to join the project, we started to have workshops together, we did a mood board with colors and things we liked, we created some sketches in paper. This was probably the hardest part of the project, but at least after some months we had some starting sketches on how we wanted to proceed!

### Allfons
The name, we spent so much time into this, what should the name be? we only had the working name Rulebooks. We met on a pub to brainstorm names, we got a huge list of names! Then we said we have to pick 10 each, then we voted and voted and something more? We finally managed to get us a name **Allfons**.

## Vacay 🌴
We had the functionality, the design sketches and the name! Now it was summer and we all was going on vacations so Allfons had to wait!

Unfortunately, after the summer my friend had new jobs, life situations and could't spend time on the Allfons project! It was hard to fail but it was also a lot of fun to create all this together with friends!

The Allfons Azure devOps still exists with a **ghost feeling** with PR's from a different time still waiting to be aproved!
