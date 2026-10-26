# iPhone Duo Group Lab — Day 2 (mid-September 2026, before Xcode 27.1 shipped)

Source ID: **LAB-2** — https://developer.apple.com/videos/play/meet-with-apple/286/ · Panel: UI frameworks engineer (bars), Music/Podcasts/iTunes Store engineer, Phone/FaceTime engineer, HI design-systems designer

Spoken Q&A from Apple's iPhone Duo Group Labs (Meet with Apple). Text comes from the English subtitle track, fetched 2026-10-26, trimmed of speech disfluencies; wording is otherwise the panel's. These are spoken answers, not documentation: hedged answers are marked *hedged* and must not be presented as fact; where a written Apple page covers the same point, prefer it (`apple-get-ready-prepare.md`, `apple-preparing-your-app-for-iphone-duo.md`, `hig-designing-for-iphone-duo.md`).

## Questions

- App full-screen on the inner display, user closes the device — does it move or get backgrounded?
- Simulating before the Duo simulator existed
- Touch input from both displays at once in the L pose?
- Tab items with text labels only, no icons?
- Custom bars
- Duo landscape vs iPad landscape — how much should the UI differ?
- Inner display in tent/standing pose for non-camera apps (alarm-clock glow)?
- Duo-only app?
- iPad-only apps in a compatibility mode?
- Preserving navigation/UI state when switching displays
- One thing to do before the Duo simulator
- Camera apps with fake auto-rotation (portrait-locked, rotating elements individually)
- How do I know the pose (laptop/desktop)? Do I need hinge APIs? How does `AVPlayerViewController` do it?
- Hardest first-party app to adapt?
- Scene accessories on the outer display — animation/update-rate limits?
- Audio mixing with two instances in Split View?
- Keeping content away from the hinge without hard-coding dimensions
- External monitor / Stage Manager?
- Asymmetric insets in Split View on the hinge side; SwiftUI layout guides
- Testing hinge poses in the simulator
- Sheets with controls (Find My-style)
- Compression and overflow
- Does iPhone Duo have its own idiom?
- Widgets on the lock screens?
- New APIs for foldable layouts; do they work on other phones?
- Speed round

### App full-screen on the inner display, user closes the device — does it move or get backgrounded?

"It'll move to the outer display... you close the device and it's just like the app has gotten smaller." Split View edge case (*relayed, "my understanding from" colleagues*): "The system will try to promote whichever one you are interacting with to the outer display. And the other one will be backgrounded... If you then open iPhone Duo again, if it was rather recently that you closed it, they'll both come back. But if it's been a while... the app they were using on the outer display [takes] the whole inner display." Timing not shared.

### Simulating before the Duo simulator existed

"run your app on iPad and just turn on window resizing... make sure you follow best practices for window scenes usage... don't use the main screen... do it through the window scene." Plus iPhone Mirroring for iPhone-only apps. (Superseded by DUO-PREPARE Step 3.)

### Touch input from both displays at once in the L pose?

"I think no." (*hedged*; the only two-display case is the camera accessory.)

### Tab items with text labels only, no icons?

"if you're only using text labels and you haven't set an icon that will still be able to go vertical. However... by default the tab bar shows the icon representation, and when someone interacts with it, that's when you'll start seeing the labels. So... it might be a little bit confusing." Design: "making those icons happen... We have a very big library of SF Symbols... over 7,400." Behavior to know: "when a person is scrubbing the tab icons in the bar on the side, the labels do appear."

### Custom bars

"first consider if you can use the system one... You do not want to get into the business of building your own. But if there's some reason that you absolutely can't. One API we have that is new in 27.1 is the ability to grab out the region that the system bar would draw in... you can request, hey, I want a bar that is, you know, like the trailing bar, and then you can know where you should put it." Music and Podcasts use that region: "music specifically to place the lyrics, the voice attenuation button... and the translation button." *[note: the API was not named on stage; the closest documented surfaces are `toolbarVerticalEdge` / `verticalBarEdge` and reserved regions — see `api-index.md`; do not present an unnamed API as a specific symbol.]* Later: "even if you're using `UITabBar`, you're also not going to get the adaptation to the vertical axis... if you're using `UITabBar`, `UIToolbar`, `UINavigationBar`, that is just going to stay horizontal. It will never go vertical... you need to use the containers." And: "we generally have APIs to place things in the correct axis, but we don't necessarily have APIs to make them behave the same... showing the labels on tap... lensing when you're scrubbing... There's also a lot of complexity in the compression logic."

### Duo landscape vs iPad landscape — how much should the UI differ?

"space availability first... start with size classes, design for that. And then what differs will be platform standards between iPad and iPhone." — "a lot of people's natural reaction was to pull those iPad designs. And we had to do a lot of work to... be like, okay, this is now the iPad design, but this is a phone... let's make sure it feels like a phone... potentially strip out anything or change it, guard it, maybe behind an idiom if it feels like it should belong only on iPad." Phone app example: "on the iPad phone app, we have the dialer in a popover... this is a phone. And so we... promoted the dialer to its own tab." Music: "the sidebar is probably the biggest difference, but we weren't making any assumption... is this an iPad or is this an iPhone."

### Inner display in tent/standing pose for non-camera apps (alarm-clock glow)?

"the answer is no... two displays on at the same time... is generally the camera accessory, which is really only for camera apps." The alarm glow "is provided by AlarmKit." Tent pose in general: "for most cases, just like your closed landscape view, outer display landscape view, just kind of propped up on a table for glanceability... it would just be your standard layout."

### Duo-only app?

*hedged* ("I have no idea") — design answer: "why would you want your app to run only on this device... you just support the whole lineup." Designing for one pose only: "no, probably not. People are going to use the device as they want to."

### iPad-only apps in a compatibility mode?

*hedged* — guidance: "make the app universal and [add] iPhone to the list of supported platforms."

### Preserving navigation/UI state when switching displays

"if you're using system navigation controllers, they will adapt for you. It's not like your app is getting created on one display and then torn down and put on another display. It's really that you're getting resized." Music's one Duo-specific behavior: "if you open the device, we put you into the lyrics mode because of course we have more space... If you close it, we put you back to normal." SwiftUI state-loss warning: "in a compact size class, they will use one container on one side of an if statement, and then they'll use a different container on the other side... guarded by size class. So if you were doing that, you will see state loss. If you haven't raised everything up... Consider this as an opportunity to see if you actually need to be doing that."

### One thing to do before the Duo simulator

Resizing on iPad / iPhone Mirroring; "asymmetric safe areas and content margins... if you've got an app that has been locked in portrait mode, you might only be checking top and bottom safe areas... test your app in landscape." — "people will check one side and say, oh, the other side must be the same... Look for a two x on a width. `.left` times two. Do not."

### Camera apps with fake auto-rotation (portrait-locked, rotating elements individually)

Two APIs: the rotation coordinator ("`AVCaptureDevice` rotation coordinator... will help you figure out the right rotation to do for your camera view") and the direction API ("Since there's two front cams, you may have previously made assumptions that the front cam is the one facing you. But that's not always true... that API will tell you... what direction the camera you're using is facing"). → TT-CAMERA; `hinge-multitasking-scenes.md`.

### How do I know the pose (laptop/desktop)? Do I need hinge APIs? How does `AVPlayerViewController` do it?

"system containers will adapt... my first question would be, what are you trying to do there? Because the guidance is you want your app to feel similar across poses. It's adapting, but it's not massively changing... one of the new APIs that I would think about is `ArrangementView`s, where it's a little bit more semantic." Music: "I've been using the reserved region to know where the division is... arrange your layout whenever it is active or not." Design: "Your layout probably in 90% of the cases, does not need to do anything special in those poses... I wouldn't overoptimize straight out the gate... when it's partially folded, is this going to be a dominant way people use this phone? Or... a transient moment... We don't know. We're all going to learn together."

### Hardest first-party app to adapt?

iTunes Store: "it took me probably just a week to move to window scenes... and to adopt Liquid Glass... just those two actually helped a lot with iPhone Duo experience by itself. We didn't do any pose specific stuff." FaceTime: "direct them towards looking at that [inner] camera because it is kind of in a unique location on the right side... we tried to put the most gravity in the UI towards that area"; the self-view PiP dodges the inner camera via the reserved-region API — "that can come and go... when the camera turns off, it's not there anymore."

### Scene accessories on the outer display — animation/update-rate limits?

"You get a full UI scene... It's not like widgets where you have to be careful about what content you can put in them." Control: "you do have to have an active camera session in order to turn on the scene accessory. But when the camera is on, you can freely control that... add a button in your UI that toggles the accessory off and on... observe when the accessory is available... if you're in Split View, when the iPhone Duo is open, then you also can't use scene accessories."

### Audio mixing with two instances in Split View?

*hedged* — "two scenes of a single app... I would expect it to not be possible to control them separately... try it on iPad."

### Keeping content away from the hinge without hard-coding dimensions

"Please don't hard code layout specific dimensions... alerts is a great example... We've done a lot of that for you. However, if you do have components that sit in the fold that are in the content area... a `UIButton`... revenue driving actions... buy now... add to cart... start free trial... Those things do need to move. So those are perfect ones to use reserved regions. If you ever want to move just a single element, that's the perfect API."

### External monitor / Stage Manager?

External display: "it does work" (*relayed*). Stage Manager: "I think the answer there is going to be no."

### Asymmetric insets in Split View on the hinge side; SwiftUI layout guides

"when it moves to the left... there will be a left safe area, but then you won't see it on the right." SwiftUI "there is a new API for that... a modifier that you can put on... that'll apply the margins for you" (*name not recalled on stage; unverified*). UIKit: "there are layout guides that you can attach to. And so just make sure you're doing that on all four edges." For apps without bars that "really just want to know if there's a camera there or if they need to respect the status bar. You can look at the corner adaptation and so then you can just respect that specific region and then you can draw everywhere else... if you're a full screen game app." Outer-display corners "don't have the same radius... the ones away from the hinge are rounder"; "it's also the only place that we have a bidirectional status bar" (horizontal in inner-portrait, vertical in the side bar).

### Testing hinge poses in the simulator

"you can change the hinge angle, and then you can also put it into the different poses... book, open, closed, laptop, tent... take a look at the transitions between poses." New in 27.1: "the simulator will launch apps that use the camera. It won't show anything in the camera view. It will detect that there are no cameras available."

### Sheets with controls (Find My-style)

"whether your sheet has or does not have controls laid out to the side, the controls kind of stay with the sheet... If you have it, you'll probably continue to have it. If you don't, you'll probably continue having it horizontal." Writing tools / Notes sheets: "a vertical bar kind of felt distracting." Find My observed live: portrait outer → "tab bar on the side and the sheet half open"; rotate → "tab bar at the bottom and the sheet on the leading side. So you don't want to cover the map." — "Find My has chosen to opt in to a different width. And that kind of comes with different choices on your bars."

### Compression and overflow

"The height of the vertical bar on the edge can change so much. Rotating the outer display, showing a video PiP above the display, moving to Split View multitasking." — "we've also never had bars sitting together... now everything's pretty much on top of each other... even on a regular height, will just now start compressing." — "if your app maybe hasn't had to handle compression or overflow menus before... How does it behave when items move into the overflow? ... set priorities so that the system can preserve the things that are most important for the longest amount of time."

### Does iPhone Duo have its own idiom?

"It is a phone... it responds to a phone idiom... It does have its own unique combination of size classes on the inner display... regular, regular, which is the first time a phone has had that." — "if you had something that's like iPad idiom, so I'm going to show the iPad UI versus phone idiom, so I'm never going to show the iPad UI... start looking at size classes now."

### Widgets on the lock screens?

*hedged* — "we have the new extra large ones on the inner display. I do not believe that those can exist on the outer display... They're six by four." StandBy works off-charger on this device.

### New APIs for foldable layouts; do they work on other phones?

`ArrangementView`: "almost every single [first-party] app has an ArrangementView." — "It does actually work on our other hardware as well. As long as the conditions that you've set up the ArrangementView are true on the device that it's running on... if you still have space for two columns, it will render as two columns. If you don't, it will move towards a single view, which is exactly what happens when you move between inner and outer display." — "good functionality should not be behind idioms."

### Speed round

Portrait vs landscape on the outer display: "why do you need to know that?... On the outer display, the size classes are different in the two axes." Hinge angle: "with the hinge interaction you can get the angle" (unit not recalled; UIKit DocC says radians, SwiftUI uses `Angle`). Web: "You can't specifically check whether it's a Duo... there is standard web API for checking whether a device has a fold and is folded." Back-deployment: "an availability check... make the stuff that is Duo specific... have an iOS 27 availability check"; for `ArrangementView` "maintain using that existing layout... when ArrangementViews are available on those operating systems, switch." Multi-window: "It does. We call it Split View multitasking."
