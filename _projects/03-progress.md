---
title: "Progress"
permalink: /projects/progress/
header:
    teaser: assets/images/progress2.png

excerpt: "My first adventure with SwiftUI."
---

![image-left](/assets/images/progress3.png){: .align-right}

Progess was my first attempt at SwiftUI, and while it remains unfinished, it was a great first learning experience into declarative programming.
{: .notice}

## What problem does Progress solve?
At the beginning of the year, I wanted to be able to track my progress reading through the Bible. Because the Bible is a collection of 66 books, it's unlikely that I would read through them in order, and so I wanted a checklist that I could complete out of order, and would visually show my progress.

In the past I'd used [Things](https://culturedcode.com/things/) to do this, but I wanted something that displayed the data in a more visual way.

The other task completion apps that I looked at didn't seem to be able to handle nonsequential progress well.

## Features
My core concept was segmented progress circles that could be tapped to open up and display a grid of targets for completing each chapter of a book.

Similarly to [Loopify](/projects/loopify/), once I'd figured out a basic layour structure, the UI was relatively straightforward.

Where I ran into issues though, was in opting to use a CloudKit-synced Core Data store for my data model. Trying to understand CloudKit and SwiftUI at the same time was tough, and that took some of the wind from my sails as I continued with the project. This was a lesson in only focussing on a single feature at a time. Because I had visions of a watch app, widgets, shortcuts, and cloud sync, I was trying to build my model with all of this in mind. I think in reality what I should have done is taken things a step at a time which is what I did with Loopify.

Progress has (ironically) been put on hold, but this is due to my busyness, rather than any stall on the project. I hope to continue with the project, but with a lower bar for version 1.0. There's no harm in building *something* that works, and then having to refactor at a later date for a new feature.