# iPhone Duo Group Lab — Day 3 (about ten days before launch, mid-October 2026; Xcode 27.1 already out)

Source ID: **LAB-3** — https://developer.apple.com/videos/play/meet-with-apple/287/ · Panel: two HI designers (system; Messages/Files/Measure), two UI frameworks engineers (tab bars/menus; bars/navigation)

Spoken Q&A from Apple's iPhone Duo Group Labs (Meet with Apple). Text comes from the English subtitle track, fetched 2026-10-26, trimmed of speech disfluencies; wording is otherwise the panel's. These are spoken answers, not documentation: hedged answers are marked *hedged* and must not be presented as fact; where a written Apple page covers the same point, prefer it (`apple-get-ready-prepare.md`, `apple-preparing-your-app-for-iphone-duo.md`, `hig-designing-for-iphone-duo.md`).

## Questions

- Designing one app across poses
- Which side should a custom view move to when avoiding the fold?
- Custom two-pane split with `GeometryReader` keeping a stale width after rotation
- Do we need physical hardware?
- StandBy, widgets, Live Activities
- Camera apps: is opening the device a camera-direction change or a layout transition?
- Seated pose stability and control placement
- Top three fixes for an existing iPhone-only app (panel's combined list)
- CI / UI tests: no `simctl` or `UIDevice` hooks for hinge state
- WebKit
- Games in the half-folded pose (gameplay top, controls bottom)
- Preserving an unfinished form / scroll position when one column becomes two
- Is the scene or window recreated on fold/unfold? Do `onAppear` / `onDisappear` / `scenePhase` fire?
- Fully open and flat: still avoid the middle?
- Tab bar vs sidebar on the inner display (TV uses a tab bar on Duo, sidebar on iPad)
- Long `UITextView` reflow on unfold — is reading position kept?
- Navigation bar large title and bar items scrolling away
- Picture in Picture
- Minimum work for a custom floating tab bar

### Designing one app across poses

"you're making one app for the iPhone, whether it's an iPhone Duo or the iPhone Pro... the same type of controls... the same kind of hierarchy, the same kind of navigation... continuous as you move between it. So people know where to look for controls." — "within a single phone, your app's going to get used in a lot more states than you might for just other phones."

### Which side should a custom view move to when avoiding the fold?

"we tend to avoid more to the right because we found that more people are more likely to hold it that way... But... other elements that are maybe smaller... you would almost want to avoid it towards the side that it is mostly on... context menus and the edit menu for text editing... will tend to stay on the side that the source view is on." — hierarchy drives it: "primary on the left and the detail on the right... things are already placed in an appropriate position, and then they can just shift over." Seated pose: "keep the interactive controls closer to the bottom... a stable surface to press against." Closing the device: "if you have a primary and a detail layout, oftentimes that detail will be the thing that you see when the device closes... because the detail is the furthest down the hierarchy."

### Custom two-pane split with `GeometryReader` keeping a stale width after rotation

"there's `ArrangementView`s, which you can style to be a split arrangement and... you can customize that ratio of the columns. And in a lot of cases that can eliminate things that you might have previously been using a `GeometryReader` for... it also will work on other iPhones." Stale sizes from `GeometryReader` on rotation: "worth filing Feedback."

### Do we need physical hardware?

"No, you don't need a physical iPhone Duo to test... there is a great Simulator experience that you can use to end-to-end test your app in all of the different states and poses." Exceptions: camera streaming, hardware-specific APIs, and feel ("You might make different decisions on a physical device"). "the Simulator that's in the betas right now doesn't have most of the first-party apps in it."

### StandBy, widgets, Live Activities

"all the different sizes of widgets that we support in StandBy for other phones directly translate to the Duo. We've introduced a new... extra large... basically fills like half of the Duo screen when it's open... not just for the Duo, but for other devices too." Book pose: "time and date and calendar on one side and then a Live Activity open on the other side."

### Camera apps: is opening the device a camera-direction change or a layout transition?

"It's not an either or. It's a both." Rear camera: "keep the cameras where they were... the customer probably just wants more screen real estate." Front camera (video calling): "the switch from the closed display camera to the internal camera... should happen automatically if you're using the right APIs... you really need to be using `AVCaptureDevice` RotationCoordinator and DirectionCoordinator." Aspect choice: "when the iPhone Duo is open, the camera on the back is still a portrait aspect ratio, right? But the screen is in a landscape aspect ratio... do you want to show a portrait preview inside of the landscape screen with letterboxing, or do you want to zoom it up and fill the screen?... in the Camera app, the default is the camera preview is center frame... in AR... ARKit, we went edge to edge and zoomed in."

### Seated pose stability and control placement

"it's sturdier than you would expect"; still, "placing controls where you may be really using them frequently on the bottom can not only be good from a device position perspective... but also just from a reachability standpoint." Three things to weigh: "If there is primary content, like a video, you probably want that included. So shifting the controls to the bottom makes a lot of sense. There are benefits just from interacting on the tabletop surface... But then the third thing to consider is we try to avoid having controls move significantly when you go between poses... start with as little as possible, unless there's enough value that it's worth it in that new pose." — "trust your users because I think they'll gravitate towards the best way to use your app."

### Top three fixes for an existing iPhone-only app (panel's combined list)

1. "adjusting for the asymmetric safe area and making sure that you don't have content that's going under horizontal safe areas or hard to reach."
2. "making sure that you have the right controls showing up in the vertical layout of the toolbars and navigation bars... that you don't have controls that are showing up without icons."
3. "readable content margins... whether you're just adding additional margin in those landscape poses so things aren't too wide to read or you're finding additional columns to show." — "lines of text being across the full width of the screen will look a little bit wonky... We have this concept of readable content width."
Also: single `NavigationStack` apps → "maybe switching to more of a `NavigationSplitView` so that you can show more of your hierarchy, like Mail"; horizontal carousels sized from "the device width, subtracting maybe some padding" → "capping to some aspect ratio... Instead of just enlarging the content, you now just surface more content"; portrait-locked apps carry hidden assumptions; "`UINavigationController` also supports toolbar items at the bottom, and those are the ones you want to use" instead of a positioned `UIToolbar`; bars on Duo "share the space with the status bar in both portrait and landscape when it's open... And when there's a Live Activity coming and going, how are you going to resize and move out of the way"; "an overflow menu inside their overflow menu" → use the system overflow APIs; custom toolbar views "the system isn't able to move that to be vertical automatically, because it doesn't know how wide it needs to be... if you're using a custom view for something that's effectively an image... just set the image on the item"; `axisBehavior` "prefers vertical... doesn't mean it will go vertical on other phones. It will still stay horizontal... you don't need to do anything that's conditional."

### CI / UI tests: no `simctl` or `UIDevice` hooks for hinge state

"We can't talk to future plans... file Feedback so that they get exposed" with the use case (snapshot testing, transitions). iPhone resizing on a Mac remains useful.

### WebKit

*relayed*: "My understanding is that the web standards include support for hinge position and that WebKit supports that."

### Games in the half-folded pose (gameplay top, controls bottom)

"You absolutely can... the best thing to use for that would probably be an `ArrangementView`. If you use the overlay mode... it can automatically stack the items on top of each other when you're not in that kind of pose. But then when you are in that kind of pose, they can show side by side with the controls on the bottom and the content on the top. Very similar to... AVKit video players... you can read if they're stacked on top of each other or not" (`overlayArrangementZIndex`). Precise avoidance for custom bars: "reading the corner adaptation... I have like this vertical safe area on the whole side, but actually I really only want to... avoid it a little bit more precisely." — "also works on iPad, because the window controls on iPad also take up the space... And Mac Catalyst as well." (`UIView.LayoutRegion.safeArea(cornerAdaptation:)`, SDK-verified — `api-index.md`.) Games generally: "making sure your game supports any aspect ratio and display size... when you are in those poses, for example, it's placed on a surface almost like a laptop. That could be a great time to move your controls down into the bottom area."

### Preserving an unfinished form / scroll position when one column becomes two

"using a navigation container or something like an `ArrangementView` that is going to preserve your state across the transition... there's nothing in doing that that is intrinsically going to tear your app down... However, that will change the size class. And... sometimes you may have a big if statement at the root of your app that is switching on: if compact, if regular, that is going to tear down all the state." Design: "when you're introducing that second column: are there any surprise moments... in Mail, we might choose to show the message that was open last. For another app... It might be surprising." — "sometimes you don't actually want to automatically pick one... opening it up and it would select an unread conversation, sending a read receipt, you probably don't want to do that... we do have a content unavailable view."

### Is the scene or window recreated on fold/unfold? Do `onAppear` / `onDisappear` / `scenePhase` fire?

*hedged on the details* — "I would expect `onAppear` and `onDisappear` not to get called again... your app would be moving to a different screen. However, your apps, your view controllers, your views, all those instances should remain the same... treating this as an adaptive layout resize... They're not different copies of your app on the other screen and the inner screen." One real `scenePhase` case: "If your app wasn't the one that was most recently interacted with [in Split View], it can go into the background when the device is closed." Also: "I would recommend not doing `UIScreen.main`. It will not work well" → the Xcode 27.1 modernization skill.

### Fully open and flat: still avoid the middle?

"It can go either way... apps where there is a natural two-column layout, it's preferable to have the two-column layout because then that partial fold transition isn't as drastic... `NavigationSplitView` or `UISplitViewController`... You'll often have a 35/65 layout. And then when you go into the folded state, then you'll have something more like 50/50... what you don't want is a very large layout transition when you go from the flattened to the folded state." Carousels "scroll right on through" the hinge. Scroll direction: "scrolling perpendicular to the hinge can work pretty well... when you're scrolling is parallel to the hinge, it can be a little bit more difficult... very difficult to reach in and actually perform a scroll right in that hinge area." Quick Look PDFs scroll horizontally in book pose. Technical trick: "request for the region ahead of time... requesting the reserved regions and asking for the inactive option" so a 50/50 layout needs no transition when folding (`.includeInactive`). App Store article in landscape (hero left, article right) avoids the fold; TV in portrait does not ("you would only have half a screen to scroll in") — "in open portrait, you might make a different choice than when it's open landscape."

### Tab bar vs sidebar on the inner display (TV uses a tab bar on Duo, sidebar on iPad)

"really depends on what you're prioritizing... apps more content-driven... navigating, looking through the content of the app versus navigating the different destinations." — "even the smaller-sized iPads still have larger displays than the iPhone Duo." — "The difference on the Duo is you're working with two sizes of the app... trying to be consistent between the closed and open states becomes a factor... the apps that I feel work pretty well for the sidebar tend to be ones where in the sidebar they're representing a view that would be available when it's closed. So you would see that in like Settings." — "I launched TV not because I want to think right now."

### Long `UITextView` reflow on unfold — is reading position kept?

"it does actually already baked into `UITextView` and various other `ScrollView`-based components. We have `UICollectionView` stable views... as your view resizes, we do actually anchor it to the visible position." Inner display regular/regular: "Yes."

### Navigation bar large title and bar items scrolling away

"on iPhone Duo, by default, the bars do actually scroll away when, I believe, you're closed and also maybe in landscape, but there is an API on `UINavigationItem` as well as toolbar behaviors... in SwiftUI, that will allow you to change that behavior... this API isn't just exclusive to the Duo... you can force it on other phones or you can force it off." *[note: API not named on stage; unverified here.]* Design: "depending on the orientation of your `ScrollView`, you may want to flip the direction that the navigation bar scrolls off... in Messages it scrolls off when you're scrolling back in the history." Rationale: "iPhone Duo has a shorter display than other iPhones. And so it can be really valuable to maximize that content area."

### Picture in Picture

"works the same way as other iPhones... The new feature... if you're in Portrait Open, there's a new control that lets you pin the video to the top of the screen... you can launch another app and use it at the bottom of the screen, and the video won't occlude the app."

### Minimum work for a custom floating tab bar

"two APIs to check out are first, you have the ability to see what edge the vertical bar is laying out on [`verticalBarEdge` / `toolbarVerticalEdge`]... and you also want to take a look at the ability to get the bar region to see where the system would be drawing... take your horizontal bar and then move it into that vertical region based on whether or not there's a vertical bar." Alternative: "bar button items support having a custom view... you're okay with having the system glass and the system laying that out, that can be a way to reduce the workload." *[note: the "bar region" API is again unnamed on stage; see Day 2 note.]*
