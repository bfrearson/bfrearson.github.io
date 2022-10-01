---
title: "StreamCam"
permalink: /projects/streamcam/
header:
    teaser: assets/images/streamcam_rounded.png

excerpt: "Mirror your iOS device's camera to your Mac."
---
<a href="https://bfrearson.github.io/streamcam/" class="btn btn--primary">StreamCam User Page</a>  <a href="https://apps.apple.com/us/app/streamcam/id1538237783"><img class="appstorebadge" src="/assets/images/appstore.svg"/></a>

StreamCam allows Mac users to obtain a high resolution video feed of their iOS camera with no obstructions. Although it's now been [sherlocked in MacOS Ventura](https://www.apple.com/uk/macos/macos-ventura-preview/), it was a valuable tool during the pandemic.
{: .notice}

## What problem does StreamCam solve?
In 2020 our church was looking for a way to stream live video to our congregation stuck at home. We (naively) believed this would only be for a few weeks, so we didn't want to spend too much on a camera, and we were looking for a solution that could use the good enough cameras in our iPhones.

Connecting an iOS device to a Mac will provide a high resolution screen mirror, which I wanted to use to display the output of the device's camera. Unfortunately there's no way to remove the UI overlays of the [stock camera](https://youtu.be/Ra9NWb2iHWE?t=765), so I began working on a solution using `AVFoundation`.

The only function of the app is to provide an unobstructed camera viewfinder, that the user can manipulate in a Mac application such as OBS.

## Building StreamCam
Although I'd learned Swift as a language a few years earlier, this was my first real project.

The initial viewfinder was very easy to implement using boilerplate `AVFoundation` code and `UIKit`, and so the main challenge was figuring out an interface that would be *intuitive* to the user, but show *nothing* on screen when using the app.

Users needed to be able to switch between the front and rear cameras, and on devices that supported it, switch between camera lenses.

<figure style="width: 350px" class="align-center">
  <img src="/assets/images/streamcam_controls.png" alt="user interface">
  <figcaption>User interface</figcaption>
</figure> 

The final interface always appears on app launch, but disappears with a tap. Users can double tap to switch between front and rear cameras, and single tap to focus as normal. There's a concise help screen with customised SF Symbols icons to give users an overview of the control scheme.
<figure style="width: 350px" class="align-center">
  <img src="/assets/images/streamcam_help.jpg" alt="help screen">
  <figcaption>Help screen</figcaption>
</figure> 

StreamCam is a niche tool, but has consistent traction in the App Store, although I expect that to tail off with the release of [Continuity Camera](https://www.apple.com/uk/macos/macos-ventura-preview/) in MacOS Ventura.