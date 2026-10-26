# Apple — "Get ready for iPhone Duo" hub and "Three steps to make your app shine on iPhone Duo"

Source IDs: **DUO-HUB** (https://developer.apple.com/iphone-duo/), **DUO-PREPARE** (https://developer.apple.com/iphone-duo/prepare/), **NEWS-1005** (https://developer.apple.com/news/?id=kkphp5qo), **ASC-SHOTS** and **ASC-PREVIEWS** (App Store Connect Help specification pages). Fetched **2026-10-26**. Apple's wording is reproduced verbatim below; headings are Apple's. Commentary is marked *[note]*.

## 1. What the hub page lists (DUO-HUB, 2026-10-26)

- Videos: the six Tech Talks (`sources.md` TT-*), plus **iPhone Duo Group Lab | Day 1 / Day 2 / Day 3** (Meet with Apple 285, 286, 287 — see `group-lab-qa.md`).
- "iPhone Duo Workshops — Join us around the world for workshops to optimize your apps and explore resizability for all screen sizes."
- "iPhone Duo Q&As — Browse questions and answers from the iPhone Duo Q&As on the Apple Developer Forums." Links: Photos & Camera Q&A, SwiftUI Q&A, UIKit Q&A (forum activities 1659080, 1664080, 1670080) and "Group Lab highlights" (forum thread 847644). *[note: the forums sit behind a human-verification page and could not be fetched; see `sources.md`.]*
- Tools and documentation: "Xcode 27.1 — Get the latest SDKs to build, run, and test your app for iPhone Duo." (no longer "beta"), HIG "Designing for iPhone Duo", Apple Design Resources, "Preparing your app for iPhone Duo", "Three steps to make your app shine on iPhone Duo".
- "Stand out on the App Store — Explore best practices, templates, and specifications for App Store assets like app previews, screenshots, product page headers, and more."

## 2. "Three steps to make your app shine on iPhone Duo" (DUO-PREPARE, full text)

> Apps built with the iOS 27 SDK and later resize dynamically on iPhone Duo. Unfold it and your app expands to fill the inner display; fold it and your app returns to the outer display. Hardcoded screen sizes, fixed orientations, and device assumptions can break layouts, leading to stretched, clipped, or misplaced content. Follow these steps to make sure your app is ready for iPhone Duo.

### Before you begin

> While all apps will run on iPhone Duo, building with the latest SDKs let your app use more of the display and adapt gracefully across layouts. The following examples show what your app will look like on iPhone Duo depending on which SDK you build with.

| SDK | Apple's description |
|-----|---------------------|
| **iOS 26 SDK** | "Apps built with the iOS 26 SDK or earlier run on iPhone Duo, but won't adapt to all layouts. When the device is open, your app will appear in the center of the display, with empty space surrounding the window. When the device is closed, the outer display will show your app's content to the left of the status bar and camera." |
| **iOS 27 SDK** | "Apps built with the iOS 27 SDK resize to fill most of the inner display on iPhone Duo, but still avoid the status bar on the right edge. This provides an improved experience, as empty space is minimized." |
| **iOS 27.1 SDK or later** | "Apps built with the iOS 27.1 SDK or later are optimized for iPhone Duo. Apps will extend to use the full display, with toolbars and tab bars appearing vertically below the status bar. This provides people with an optimal and consistent experience as they move across displays." |

> Before shipping an app build with the iOS 27.1 SDK, make sure your app's content adapts to the bars becoming vertical.

### Step 1: Run the app resizability skill

> In Xcode, ask the coding assistant to "get my app ready for iPhone Duo." The skill scans your project for the patterns described in Step 2. For each issue it finds, it explains the problem and suggests a fix.
>
> If you use a different coding agent, export the skill by running this in Terminal:
>
> `xcrun agent skills export`
>
> The skill finds most issues, but not all of them. Use the list below to review what it flags and catch anything it misses.

*[note: on Xcode 27.1 (27A9275) the command requires `--output-dir <absolute path>`; see `apple-app-resizability-skill.md`.]*

### Step 2: Fix patterns

**Stop sizing from the main screen**

> On iPhone Duo, your app's size changes whenever the device folds or unfolds, and it doesn't always match the size of the screen. Search your project for `UIScreen.main` and replace each use:
>
> - **Size from the view or window.** Replace `UIScreen.main.bounds` with `view.bounds` or `window.bounds`. In SwiftUI, use `GeometryReader` or `onGeometryChange`.
> - **Re-read the size whenever it changes.** Reading the size once at launch or in `viewIsAppearing` isn't enough, because a fold can resize your app in the middle of a session. Do size-dependent layout in `layoutSubviews` or `viewDidLayoutSubviews`, and use `viewWillTransition(to:with:)` for work that needs to run when the size changes.
> - **Get scale from the trait collection.** Use `traitCollection.displayScale` in UIKit or `@Environment(\.displayScale)` in SwiftUI.
> - **Create windows from a scene.** Replace `UIWindow(frame: UIScreen.main.bounds)` with `UIWindow(windowScene:)`.
> - **Don't store a `UIScreen`.** When screen information is needed, access it dynamically with `view.window?.windowScene?.screen`.

**Avoid using orientation to make layout decisions**

> On iPhone Duo, the device's orientation doesn't tell you what shape your app is. Search for these properties and remove any layout logic that depends on them:
>
> - `UIDevice.current.orientation`
> - `statusBarOrientation`
> - `interfaceOrientation`, including `windowScene.effectiveGeometry.interfaceOrientation`.
>
> To find out whether your app is wider than it is tall, compare the width and height of the space you have: `view.bounds` in a view controller, `superview.bounds` in a view, or the size from `GeometryReader` in SwiftUI.

**Stop assuming idiom or size class identifies the device**

> With iPhone Duo and resizable iPhone Mirroring, an iPhone app can have any combination of size classes. On the inner display, your app should look like a natural extension of its iPad layout. For more information, see Prepare your app for iPhone Duo.
>
> - **Don't choose a layout by idiom.** Search for `userInterfaceIdiom == .phone` and `== .pad`. Base layout choices on size classes or available space instead.
> - **Don't treat "regular width" as "iPad."** Code that shows an iPad-only layout when `horizontalSizeClass == .regular`, or that assumes iPhone is always compact, will now run that layout on iPhone too.
> - **Remove checks for specific device sizes.** Comparisons such as `bounds.height == 844` or a list of known iPhone sizes won't match iPhone Duo.
> - **Check full-bleed media.** Hero images and video set to `.scaleAspectFill` or `.aspectRatio(contentMode: .fill)` can lose important content on wider displays. Use the current size classes or aspect ratio to choose between fill and fit, or set a focal point.

### Step 3: Test resizing

> Test resizing using these tools to make sure your app runs as expected.
>
> - The iOS resizable simulator in Device Hub.
> - iPhone Mirroring on macOS 27. Resize the window to extreme sizes in both directions.
> - The iPhone Duo simulator in Xcode 27.1.
>
> Note: The iPhone Duo simulator is the best way to test the full native experience.

### Go further: Stand out on the App Store

> Update your App Store product page to show that your app is ready for iPhone Duo.
>
> - **Showcase iPhone Duo support.** Share app previews and screenshots that highlight your app across orientations. When you submit iPhone Duo screenshots in App Store Connect, your App Store product page will display a badge to let people know your app is optimized for iPhone Duo. View screenshot specifications.
> - **Preview your assets.** Use the new preview tool in App Store Connect to see how your assets — including new creative assets — look on iPhone Duo.
> - **Share your app with us.** Submit a featuring nomination in App Store Connect to let the App Store editorial team know your app supports iPhone Duo. In the Helpful Details section, you can specify that your app is optimized for iPhone Duo, including support for all device poses. Learn how to create a featuring nomination.

Additional resources listed: News "Prepare and submit your apps for iPhone Duo"; Video "Prepare your app for iPhone Duo"; Events "Sign up for iPhone Duo workshops"; Documentation "Browse iPhone Duo resources".

## 3. News — "Prepare and submit your apps for iPhone Duo" (NEWS-1005, October 5, 2026)

> iPhone Duo will be available to customers starting October 23, 2026. To get your apps and games ready today, you can:
>
> **Build or recompile with Xcode** — Recompile existing apps with Xcode 27.1, which adds development support for iPhone Duo. Use Device Hub to visualize how your apps behave on iPhone Duo across all poses and orientations.
>
> **Prepare your App Store product page assets** — Updated screenshot and app preview specifications are now available for the latest Apple devices, including iPhone Duo, so you can showcase your app at its best across orientations. Explore design guidance and get downloadable templates to help you build your App Store assets, including new product page headers and search result assets, as well as your app previews, screenshots, and In-App Events. Visualize how your screenshots, product page headers, and other metadata look on your product page on iPhone Duo with the new preview tool in App Store Connect.
>
> **Submit your app** — You can submit your iPhone Duo optimized apps and games in App Store Connect today. When you submit iPhone Duo screenshots in App Store Connect, your App Store product page will display a badge to let people know your app is optimized for iPhone Duo. Starting April 2027, any apps or games submitted will need to include screenshots for iPhone Duo.

## 4. App Store Connect specifications (ASC-SHOTS, ASC-PREVIEWS, fetched 2026-10-26)

**Screenshot specifications — iPhone Duo**

| Display | Screenshot size (pixels) |
|---------|--------------------------|
| Outer display | 1398 × 2034 or 2034 × 1398 |
| Inner display | 2007 × 2853 or 2853 × 2007 |

> Starting in April 2027, screenshots will be required for any app using the iOS 27.1 SDK or later.
> When you submit iPhone Duo screenshots in App Store Connect, your App Store product page will display a badge to let people know your app is optimized for iPhone Duo.

*[note: the news item says "any apps or games submitted"; the specification page scopes the April 2027 requirement to "any app using the iOS 27.1 SDK or later". Quote the one that fits the question; do not merge them.]*

**App preview specifications — iPhone Duo**

> Device resolutions: 2034 × 1398 pixels; 2853 × 2007 pixels (19.5:9 aspect ratio)
> Accepted resolutions: 886 x 1920 pixels (portrait); 1920 x 886 pixels (landscape)

*[note: apple.com's tech-specs page gives the inner display as 1878 × 2670 pixels, while App Store Connect gives 2007 × 2853 for inner-display screenshots. Both are Apple's published figures; they disagree and Apple has not reconciled them. Neither is a developer-documentation statement of point size or scale. See `device-facts.md`.]*
