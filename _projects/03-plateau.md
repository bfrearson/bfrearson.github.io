---
title: "Plateau"
permalink: /projects/plateau/
header:
    teaser: assets/images/plateau2.png

excerpt: "Guitar scales practice app"
---

![image-left](/assets/images/plateau1.png){: .align-right}

Plateau is a spaced repetition app for practicing guitar scale positions. Each session, the user is presented with a selection of three reps of three scales in various positions.
{: .notice}

## What problem does Progress solve?
Plateau is an idea that I developed after reading about the *OK plateau* in [Moonwalking with Einstein](https://joshuafoer.com/). The concept is that most people learn enough to be OK at a skill, but getting off that plateau is hard work.

Practicing guitar scales has been an area of my playing that has never seriously had much attention. Minor pentatonic position 1 is really the only one I can do without thinking. I've been subscribed to [Pickup Music](https://www.pickupmusic.com/) for a couple of years, and one particular exercise there sparked an idea about being able to be served fresh ides each day, while still making measurable progress.

Chat GPT is perfect for this kind of content, so I asked for some ideas for some basic exercises I could iterate through.

<figure style="width: 350px" class="align-center">
  <img src="/assets/images/chatGptExercises.png" alt="user interface">
  <figcaption>User interface</figcaption>
</figure> 




## Features
For the app, I'd been reading [Andy Matuschak's](https://andymatuschak.org/) writings about spaced repetition and so I decided to use this both in short-term and long-term ways: 
- In any given session, we iterate through three different exercises and return to each three times.
- The chosen scales and positions are selected based on a weighted record of their last occurence. Users are more likely to see positions that they have not played in a while.

The intention for Plateau is to remove as much friction as possible between guitar players and their scales, with the goal of promoting fluency across all scale positions and keys. I recognise that the pirmary factor which delivers these results is just time spent practicing intentionally, and that's all this app sets out to do.

I have thought about adding additional features, but there's something to be said for simplicity...