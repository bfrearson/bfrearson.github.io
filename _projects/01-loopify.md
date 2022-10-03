---
title: "Loopify"
permalink: /projects/loopify/
header:
    teaser: /assets/images/loopify_rounded.png

excerpt: "An intuitive UI for looping part of a song in Spotify"

gallery_design:
  - url: /assets/images/loopify_old.png
    image_path: /assets/images/loopify_old.png
    alt: "original UI"
    title: "Original UI design"
  - url: /assets/images/loopify1.png
    image_path: /assets/images/loopify1.png
    alt: "new UI design"
    title: "New UI design"

gallery_features:
  - url: /assets/images/loopify_seek.gif
    image_path: /assets/images/loopify_seek.gif
    alt: "seek gif"
    title: "Tap to seek"
  - url: /assets/images/loopify_loop.gif
    image_path: /assets/images/loopify_loop.gif
    alt: "loop gif"
    title: "Double tap to loop"
  - url: /assets/images/loopify_zoom.gif
    image_path: /assets/images/loopify_zoom.gif
    alt: "zoom gif"
    title: "Pinch to zoom"
  - url: /assets/images/loopify_scroll.gif
    image_path: /assets/images/loopify_scroll.gif
    alt: "scroll gif"
    title: "Drag to scroll"

---

<a href="https://bfrearson.github.io/loopifyapp/" class="btn btn--primary">Loopify User Page</a>

---

![image-left](/assets/images/loopify1.png){: .align-right}

Loopify is my latest app. It's currently in TestFlight, awaiting approval from Spotify to use its [Web API](https://developer.spotify.com/documentation/web-api/). If you would like access to the beta, please [contact me](/pages/contact/) with your Spotify account name. 
{: .notice}

Loopify was created over a period of 50 days, during which I documented my thoughts on my [blog](/blog/).

## What problem does Loopify solve?

I play guitar, drums and piano in my spare time. I believe that one of the best ways to improve musicianship is learning by ear, and so I try to learn new songs this way. Using a regular music player like Spotify for this can be frustrating if I have to repeatedly return to the same place in a track until I get it.

There are other apps that do this, but I've found that the UI for most of these a pretty poor. Loopify is my attempt at creating a beautiful, intuitive solution to this problem. It acts as a controller for any device that is playing through Spotify Connect, and so is not limited to the device it's running on.

## Features

The Spotify [Audio Analysis](https://developer.spotify.com/documentation/web-api/reference/#/operations/get-audio-analysis) endpoint provides both track details and information about bars and sections for songs in its library. Loopify uses this information to provide a more musical understanding of the track.

{% include figure image_path="/assets/images/loopify_trackdetails_1046.png" alt="track details view" caption="Track analysis details from Spotify." %}

My design for the app features a minimap view on the left, displaying the entire track, with sections, and a moveable playhead. The right-hand pane shows a magnified, zoomable view of the track, which allows users to focus in on specific parts. **Single tap** a section or bar to seek to that position; **double tap** to create a loop over that section. Users can fine-tune their loop using the drag handles.

{% include gallery id = "gallery_features" layout = "half" caption="User interactions" %}

## Behind the scenes

### Interface
The Loopify UI is [very-nearly]({% post_url 2022-09-28-loopify-day54 %}) entirely written in SwiftUI. This is my [second project](/projects/progress), but the first that I've seen through to completion. Declarative programming has been a challenge, but I've enjoyed it. Building the UI took a significant amount of time in the project, and I scrapped my [initial idea]({% post_url 2022-08-22-loopify-day39%}) in favour of something that I think is more refined.

{% include gallery id = "gallery_design" caption="Old vs new UI" %}

### API
The first stages of the project were spent trying to figure out which API would be the most suitable. My original concept was to have audio streaming directly to Loopify, which users able to slow down and transpose songs. This would have been a much nicer solution, but unfortunately Spotify announced they were [shutting down their streaming SDK](https://developer.spotify.com/community/news/2022/07/15/mobile-streaming-sdks-update/) just as I started work. As of September 2022 there is no way to access streaming audio outside of the official Spotify channels.

My remaining options were the [iOS SDK](https://developer.spotify.com/documentation/ios/) or the [Web API](https://developer.spotify.com/documentation/web-api/). I built out helper classes for both, but in the end the authorisation flow for the iOS app proved too janky for a good user experience: Users had to be pushed out to the Spotify app to authenticate, and it would only work if the Spotify app was playing music. Instead, I opted for the Web API, which has the added advantage of separating the audio from the device running Loopify, which allows users to control audio wherever it's playing.

I originally built out a data model and some basic methods for the Web API myself, but after coming across [Peter Schorn's API](https://github.com/Peter-Schorn/SpotifyAPI), I decided to use his more advanced, Combine-based, package. This made it easy to interface with SwiftUI's state variables.