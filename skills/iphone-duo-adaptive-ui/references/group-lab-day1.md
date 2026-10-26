# iPhone Duo Group Lab — Day 1 (mid-September 2026, before Xcode 27.1 shipped)

Source ID: **LAB-1** — https://developer.apple.com/videos/play/meet-with-apple/285/ · Panel: UI frameworks engineer, two HI designers, TV app engineer, Maps engineer

Spoken Q&A from Apple's iPhone Duo Group Labs (Meet with Apple). Text comes from the English subtitle track, fetched 2026-10-26, trimmed of speech disfluencies; wording is otherwise the panel's. These are spoken answers, not documentation: hedged answers are marked *hedged* and must not be presented as fact; where a written Apple page covers the same point, prefer it (`apple-get-ready-prepare.md`, `apple-preparing-your-app-for-iphone-duo.md`, `hig-designing-for-iphone-duo.md`).

## Questions

- Can non-camera apps display things on the outer screen while unfolded?
- Treat iPhone Duo as another adaptive size-class environment, or build Duo-specific layouts?
- Seated pose with the camera side face down — can an app detect which half is on the table?
- Will a custom tab bar at the bottom automatically adapt?
- One thing every developer should adopt?
- Minimizing letterboxing/pillarboxing on launch day; what each SDK gives you
- Will Xcode ship an agentic skill for Duo?
- App locked to portrait — does it still get size-class changes when folding?
- Accessibility nuances
- One canvas or two displays?
- First-party examples
- Three-column iPad layout on Duo?
- Safari/WebKit APIs for poses?
- Does each pose need manual adoption?
- Inner display: distinguish landscape vs portrait when both are regular/regular?
- Can I keep the regular iPhone behavior on Xcode 27 while I finish months of Duo work?
- What resizable-simulator / iPhone Mirroring testing catches vs. what needs the Duo simulator
- Full-screen-only app (e.g. immersive AR)?
- Biggest design mistake seen?
- Multiple instances of the same app?
- Core Motion reference frame on Duo?
- Also stated in the opening:

### Can non-camera apps display things on the outer screen while unfolded?

"There's a system entitlement that you can adopt in your app... the companion display for the camera is only available when there's an active camera session. So it's not enough just to have the entitlement. You actually have to be running a camera session to light up the inner and outer displays together." Later restated: "only camera apps... can use both displays at the same time, and only apps with an active camera session." Example given: Kid Cam shows "Snoopy... on the outer display to get the kids attention." → `hinge-multitasking-scenes.md` (scene accessories).

### Treat iPhone Duo as another adaptive size-class environment, or build Duo-specific layouts?

"If you're using built-in components, they work really well and adapt for you... iPhone Duo is just another part of that seamless experience of layout." (frameworks) — "Every app should really be adapting to any size and expecting any size... then if there's any particular conventions for the device you're running on... you start to layer those on too. And for Duo, that's where the vertical controls start to come in." (design) — Ergonomics: "you're using this phone out of your pocket closed quite a lot. And so all your muscle memory starts to develop around those vertical controls. So when you open it, a lot of the time, you're going to reach for that same position on the screen." — "we certainly tried to make it feel very consistent between the closed pose and the open landscape pose... when you rotate into portrait... it's more of a two-handed mode... so we've changed the layout there."

### Seated pose with the camera side face down — can an app detect which half is on the table?

*hedged.* "I believe it does lay out like you can place the device like that and the interface works and it'll look the same as it does when it's flipped 180. Whether it's possible to detect that... I'm not totally sure." Design takeaway stated without hedge: "Expect people to use this device in and through all of the different ways of holding it... you really need to be thinking about supporting all of the different ways that might be used."

### Will a custom tab bar at the bottom automatically adapt?

"You are creating a `UITabBar` and inserting that as a subview. It won't automatically be placed in the vertical bar. If you're using a tab bar controller or a `TabView` in SwiftUI, then that functionality you'll get automatically." — "there are reserved regions and ways to do that alignment, but there are a lot of places that vertical bars can appear... When the device is closed, as you rotate it, the vertical bar always aligns with where the camera is, so it can be on the left. And if you're in Split View multitasking... the left-hand app will have a left-hand bar. And so there's just a lot of asymmetry that you need to take care of." — Maps/Find My: "this year we did a lot of work to migrate all our screens over to navigation controller and tab bar controller in order to get the vertical bar." — System tab bar behavior to match if you insist on custom: "we show the labels on touchdown... when you touch down and start dragging, it's clear what all the tabs are."

### One thing every developer should adopt?

Vertical bar (Maps); `ArrangementView` — "TV is using AVKit, which uses it to move the video to the top and controls to the bottom when you position the phone like a laptop" (TV); designing for each pose (design). Frameworks engineer on choosing the API layer: "From the lowest level... you can get the actual angle that you folded to. And then... we'll give you basically a rect on the screen when you've properly folded the device to a certain angle [reserved regions]. And then on top of that, for developers who might be using lower level layout primitives like an `HStack` or a `ZStack`, we came up with this `ArrangementView` concept... And then... all of the main system components from alerts and sheets and split view controllers, all of those things adapt... figure out for the experience that you're trying to build, which is the proper API... it's fun to... say, I want the pure angle of the hinge and do some wild math... but the reserved region API might be a better fit for that, or an `ArrangementView`."

### Minimizing letterboxing/pillarboxing on launch day; what each SDK gives you

"if you're linking against Xcode 26 on the inner display, you will be letterboxed. Your app will appear in iPhone regular on other iPhone aspect ratio with black bars on the sides. Once you build with Xcode 27... you support resizability. You'll use almost the full interior screen, but there'll still be a black bar on the side underneath the status bar. And... you build with Xcode 27.1. Then you'll go to full screen." On the outer display with a pre-27 SDK: "you will see basically the ratio of an iPhone mini for your application. So there will be a letterbox on the side where the vertical bar would be. When you open up... that same ratio will be in the middle of the screen... in both landscape and portrait... you don't get any reactivity to any of the different poses." Testing tip: "resizing your app on iPhone Mirroring. You're going to be able to get an aspect ratio that's pretty close to that... because that experience also doesn't change... the type of device that the system thinks you're on." Written version: `apple-get-ready-prepare.md` §2 "Before you begin".

### Will Xcode ship an agentic skill for Duo?

"Xcode 27.1... is going to have an updated version of the modernize your app skill that's updated to support SwiftUI in addition to UIKit. And that knows about iPhone Duo." Maps used it: "we had used a little bit of it in Maps actually for... `UIScreen.main`, and it worked really well." → shipped; see `apple-app-resizability-skill.md`.

### App locked to portrait — does it still get size-class changes when folding?

"on the outer display, we will honor supported orientations, but on the inner display, we won't. Yeah, and so it will adapt. And the size class will be regular, regular on the inside." — "There's things like `UIRequiresFullScreen` and we strongly recommend that if you can get off of those APIs... There are many different types of APIs, especially in UIKit around orientation and some of those may not reflect what you may naturally think on this new display. And so we really want you to go off of the size classes." Rationale: "As soon as you go from a portrait aspect ratio to square, now it's a little bit wider. Like do you not support landscape? Where do you draw the line?" *[note: this is the clearest Apple statement reconciling the two TT-PREPARE passages quoted in `device-facts.md` → "Orientation behavior".]*

### Accessibility nuances

Dynamic Type at the largest sizes "are going to look really good when they open their phone"; use "the readable content guide"; "it's one of the first times where VoiceOver is working on two displays at once." **Reduce Transparency:** "when you turn that on, it will basically draw an opaque rectangle behind the vertical bar. And it's a little bit narrower than the safe area for the vertical bar because you didn't want it to directly touch content. And so... turning that on and making sure your app looks good with it on."

### One canvas or two displays?

"we want it to be as cohesive as possible because some tasks will be started closed and finished open... one singular thing, not a phone when it's closed and iPad when it's open... you don't have to manage windows differently between the two states or have different settings for the different states." — "when the phone is closed and you open it, you're just resizing your app to be wider... your whole app is a resizable window that you're dragging, except you're just unfolding this phone."

### First-party examples

Maps: "when we're portrait closed, we have the sheet coming from the bottom. When you open it, the sheet gets leading placement. So you see a lot more map on the right side. So it's using the iPad layout on the inner display... similar to closed landscape." Maps also reframes the map "to make sure that, say, if you're viewing a place card, it's centered in the screen." TV: "when we open, we basically show more content... in the homepage we have this hero header where we show trailers. So when we open, we can add the description of the trailer and some more info." Find My: "the tab bar was already embedded in the sheet. So we just kept that... if the tabs apply to all of the states of the sheet, then it makes sense for them to be together... But there might be a different app where a tab has its own sheet. And maybe they do make sense to be separated."

### Three-column iPad layout on Duo?

"you get two columns at a time... there's a button in the top left that shows you the root column as an overlay... if you're in portrait... looking at the Mail app, you'll see just the detail view. And if you press the button in the top left, you'll get the sidebar as an overlay and you can pull it in and actually see those two root views next to each other." Same as "a Pro Max in landscape." Components: `NavigationSplitView` / `UISplitViewController`; `TabView` / `UITabBarController` "can show the tabs as a sidebar when you're in open wide." Bonus: "when you go partially open, like a book, then the split view automatically resizes and takes up 50/50 of the screen."

### Safari/WebKit APIs for poses?

*hedged, no WebKit expert present.* "there's some existing JavaScript standards around foldable devices. I would look to those... I'm not an expert on WebKit... try it out and file feedback." → Apple still publishes no Duo web guidance; `device-facts.md` "NOT documented" stands.

### Does each pose need manual adoption?

"if you follow the best practices, the app will look great. I think you could go a step further and really think about how to optimize your app for the specific pose. If there's anything contextual to what the app is doing... the video player and the podcast player, the controls are at the bottom when it's seated on a table." Design lesson: "we kind of had this overlay of all the different poses... it almost created a goal out of filling each pose with something exciting... as we got further along, we realized there's some staples that if you support those, you get support across everything... don't force it... don't feel stressed about having to sizzle up every single pose." Transitions: "if you have one pose UI in seated pose and it's entirely different than when you're flat, you have to think about how that actually transitions. Can be disorienting." TV example: controls "slide apart. It's like your eye follows to where the pieces are going... if you use `ArrangementView`s, you will just get that."

### Inner display: distinguish landscape vs portrait when both are regular/regular?

Design: "you start getting into available width. So you might go to a two column... in the App Store... we might show two columns of things landscape and then a single column of things vertical... but it should still feel like the same content, just rearranged." Technical: "although we don't respect the supported orientations on the inner display, we do provide the current orientation in the environment and as a trait. So between the size classes and reading the orientation, technically you can differentiate." Immediately qualified: "I do think there are better ways... You can make a very thin window on iPad that is technically in landscape orientation... really thinking about what is the width you have and the content that you're trying to display... if you have these container level framework components, they're going to do the heavy lifting for you." *[note: consistent with DUO-PREPARE "compare the width and height of the space you have".]*

### Can I keep the regular iPhone behavior on Xcode 27 while I finish months of Duo work?

"There is not. Once you link on iOS 27, you get iPhone resizability." — "you don't have to on day one have the absolute perfect pose layout for every pose... app teams, even with just 1 or 2 engineers, were able to pretty quickly get something that looks great."

### What resizable-simulator / iPhone Mirroring testing catches vs. what needs the Duo simulator

"with resizability you can detect things... if you interact with the corner of the window... you're not following the correct layout guides... On the Duo, you would find more things like the vertical bar, which makes the safe area margins asymmetric... most of the issues you would find with just iPhone Mirroring." — "a lot of phone apps that have been locked to portrait have only worried about top and bottom safe areas... the unsafe areas are on the side often now." — "the content insets are also asymmetrical... use the real content insets from the system and not assume that one side is equal to the other side."

### Full-screen-only app (e.g. immersive AR)?

"it would be the equivalent of just building an app like any other app and hiding the status bar. And you've got a full screen app. You don't have to use the vertical controls if it doesn't make sense."

### Biggest design mistake seen?

"trying to design for every pose and pulling back from that."

### Multiple instances of the same app?

"It works in a very similar way to iPad... you can have multiple views of your app open at the same time... side by side with each other or... paired with other apps." Shared state: "if both instances are using, for example, app storage, do they share the same state? Yes, the same app... the same update mechanisms that SwiftUI or UIKit currently use."

### Core Motion reference frame on Duo?

*not answered* ("it's magic, you don't need to worry about it").

### Also stated in the opening:

"when iPhone Duo becomes available on October 23rd, it will run iOS 27.1"; "the Figma and Sketch design kits are available from the Apple Developer website" *[note: the Design Resources page fetched 2026-10-26 lists an iPhone Duo entry under Photoshop/PNG; no separate Figma or Sketch Duo file is named on that page. Check developer.apple.com/design/resources/ before telling a user which formats exist.]*
