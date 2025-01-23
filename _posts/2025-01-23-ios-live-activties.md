---
    title: "Interactivity with Live Activities and App Intents"
    categories:
      - Blog
    tags:
      - swiftUI
      - ActivityKit
      - App intent
---

Recently I hit a challenge when trying to build a [Live Activity](https://developer.apple.com/documentation/ActivityKit) for iOS 18. My goal was to essentially mimic the Clock app's timer activity, with buttons to start/stop the timer.

![image-left](/assets/images/apple_live_activity.jpeg)

Apple's documentation for [interactivity in widgets and live activities](https://developer.apple.com/documentation/widgetkit/adding-interactivity-to-widgets-and-live-activities) states

> To create widgets and watch complications, you add a widget extension to your project. **As a result, any code for widgets and watch complications runs in an independent process that’s separate from your app.** ... **the system can’t run your code or update data bindings at the time it renders your widget.** This is where the App Intents framework comes into play. App intents allow you to expose actions of your app to the system and enable it to perform the actions when needed — for example, when a person interacts with a button or a toggle in a widget.

> Live Activities don’t use a timeline mechanism to update their content. However, they use WidgetKit and a widget extension with a similar cycle of view archiving and decoding. **As a result, you add buttons and toggles to a Live Activity in the same way as do for your widgets, as described below.**

I've watched most of the 2023 and 2024 WWDC videos on Live Activities, App Intents and Widgets, and they are pretty adamant about using app intents as the bridge between the main app target and the widget extension target. This makes sense, and I successfully created an app intent that I could run from Shortcuts. When it comes to adopting interaction into the Live Activity or Widget, we have to use `Button(_:intent:)` or `Toggle(_:isOn:intent:)` - trying to execute a normal closure here results in no button action, and the tap returning us to the main app. This means that the only way to add interactivity is with an App Intent as far as I know.

This is no problem for button which modify persistent properties, such as adding an item to a to do list, where all we need is shared access to some on-disk storage like UserDefaults, or a shared database. Apple demonstrates this in their example projects and videos.

However, nowhere does it cover something that should only exist in the memory space of the main app, such as the state of a timer, or audio playback. Apple has considered this, but they haven't explained to us how to deal with it:

> If you adopt the LiveActivityIntent or AudioPlaybackIntent protocol, the system **runs the app intent in the app’s process. Make sure to add your custom app intent to your app target.**

This means if we add a shared `LiveActivityIntent` across both the main app target and the widget extension, only the main app's intent will be executed. This is what we want, but it presents a problem if our intent is dependent on some other structure that needs to be referenced in the main target as in the example below.

```Swift
import AppIntents

struct PlayPauseIntent: LiveActivityIntent {
    static var title: LocalizedStringResource = "Play or pause the activity"
    
    func perform() async throws -> some IntentResult {
        PlayPauseActivityManager.shared.togglePlayPause()
        return .result()
    }
    
}
```

Our widget extension will not compile if we don't include `PlayPauseActivityManager`, which is confusing, because we're now adding a dependency which by Apple's definition isn't going to be executed. The extension app target needs to know about the definition of the Intent, but it doesn't really need to understand what's being triggered there. In this small example, there's no big downside to including the manager in the extension's target, but given the requirement that widget bundles be < 4kb, I don't like the idea of having to include this. In a more complicated project, where we might have interlinked dependencies, this might not be feasible. Not to mention it's not easy to read or follow. I didn't believe this was Apple's intended way of working with AppIntents, and I spent a long time searching for a solution.

The only reference I could find for others with the same issue was a single [developer forum thread](https://developer.apple.com/forums/thread/731852) that recommends splitting out the intent into multiple files:

Shared:
```Swift
// Add this to **BOTH** the app target and the widget extension

import AppIntents

struct PlayPauseIntent {
    static var title: LocalizedStringResource = "Play or pause the activity"
}
```

Main app:
```Swift
import AppIntents

// Add this **only** to the main app target

 extension PlayPauseIntent: LiveActivityIntent, @unchecked Sendable {
     func perform() async throws -> some IntentResult {
         PlayPauseActivityManager.shared.togglePlayPause()
         return .result()
     }
}
```

Widget Extension:
```Swift
import AppIntents

// Add this **only** to the widget extension

// Placeholder to allow widget extension to compile.
// Per Apple's docs, `LiveActivityIntent` is only ever called from the main app
// so this will never actually be called
 extension PlayPauseIntent: LiveActivityIntent, @unchecked Sendable {
    func perform() async throws -> some IntentResult {
        return .result()
    }
}
```

As a result, we can now remove the dependency on our `PlayPauseManager` from our widget extension, and this frees us up to execute whatever main app target code we want in here.

I've included a link to a [repo](https://github.com/bfrearson/live-activity-demo) here that contains the most basic example I could think of - hopefully this benefits someone and saves them the hours I spent trying to dig out a solution!