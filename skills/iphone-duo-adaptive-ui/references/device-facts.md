# iPhone Duo — device facts an agent may rely on

Only facts stated by Apple are listed. Each carries its source ID (see `sources.md`). The final section lists what Apple has **not** published; do not fill those gaps by guessing.

## Anatomy (HIG-DUO "Anatomy"; TT-DESIGN 0:28)

- Two displays: an **outer display** (used when the device is closed) and an **inner display** (revealed when open). Each has its own front-facing camera.
- A **center hinge** lets people open and close the device. "The hinge also impacts the space available for your content as the device folds." (HIG-DUO)
- **Outer front-facing camera**: in the corner, always visible, "vertically aligned with controls on the side." Its reserved region "expands into the Dynamic Island for Live Activities." (HIG-DUO)
- **Inner front-facing camera**: behind the display; "stays hidden until the camera is active." Its reserved region "is only present when the camera is active." (HIG-DUO)
- Outer display shape: "wider and shorter than the display on other iPhone devices." (HIG-DUO "Best practices") / "wider and shorter than a traditional iPhone" (TT-DESIGN)
- Poses named by Apple: "partially folded like a book, placed down on a surface, or standing on its edges" (HIG-DUO); "seated on a table like a laptop with the inner display facing you" (TT-DESIGN); the HIG illustration shows "six device poses" (alt text) but does not name all six.
- Both displays have a Dynamic Island (APPLE-SPECS "Both displays"). On the outer display the Dynamic Island "expands vertically as Live Activities arrive" (TT-DESIGN).

## Size classes (TT-PREPARE 2:46, spoken transcript; HIG-DUO "Device poses")

| Display | Orientation | Horizontal size class | Vertical size class |
|---------|-------------|-----------------------|---------------------|
| Outer | portrait | compact | regular |
| Outer | landscape | compact | compact |
| Inner | (any) | regular | regular |

Quoted: "On the outer display, like other iPhone models, iPhone Duo has regular vertical size class and compact horizontal size class in portrait, and compact vertical and horizontal size classes in landscape. On the inner display, the additional space lets you show more content like sidebars. As such, it has regular horizontal and vertical size classes." (TT-PREPARE)

HIG-DUO: "A compact width layout for the outer display and a regular width layout for the inner display give you the fundamentals for every pose."

Apple does not state the size classes in Split View multitasking or with a pinned picture-in-picture video; it says to use "tools like size classes and scene geometry" for those layouts (TT-SCENES). Read them at runtime.

## Orientation behavior (TT-PREPARE, spoken transcript — two statements, quoted verbatim)

1. "The interface orientation on the outer display behaves like any other iPhone. iPhone Duo is a great opportunity to support landscape orientation, as people may want to set the phone down like a tent. Interface orientation on the inner display behaves differently. **The inner display doesn't honor your supported interface orientations.** As with Idiom, avoid checking interface orientation for layout decisions. Use size classes instead."
2. Later in the same talk: "iPhone Duo will continue to honor the `UIRequiresFullScreen` key, but your app will still resize when someone opens or closes their iPhone Duo. **iPhone Duo respects your supported interface orientations, but your app will scale on the inner display, including in Split View multitasking.**"

These two passages read differently. The Group Lab panel (LAB-1, spoken) reconciled them: "on the outer display, we will honor supported orientations, but on the inner display, we won't... it will adapt. And the size class will be regular, regular on the inside." Also stated there: "we do provide the current orientation in the environment and as a trait" on the inner display, immediately followed by "there are better ways... what is the width you have." DUO-PREPARE (written) adds the method: "To find out whether your app is wider than it is tall, compare the width and height of the space you have."   PREP (2026-09-22) adds only: "Don't use `userInterfaceIdiom` or `UIInterfaceOrientation` for layout decisions in your UIKit app." Related HIG-DUO game guidance: "You can choose to lock to either portrait or landscape orientation, but be sure to fill the screen as the device pose changes." What Apple is unambiguous about: do not build layout logic on orientation; build it on size classes. If exact orientation semantics matter to a task, tell the user this ambiguity exists rather than resolving it silently.

## Windows, scenes, multitasking (TT-SCENES chapter summaries; TT-DESIGN)

- "All apps participate in multitasking on iPhone Duo, where two apps sit side by side." (TT-SCENES 2:59)
- Split View is a "50/50 split" created by dragging an app to the side using the home gesture; "In split view, the controls sit along the outer edge." (TT-DESIGN)
- Picture-in-picture video can be pinned to the top of the screen; the current app "resizes vertically to fit the space that's left," and when partially folded "the video extends to fill half of the screen." (TT-DESIGN)
- Multiple scenes (windows) of one app are supported for the first time on iPhone; "new windows can't be created on the outer display — that's reserved for the inner display." (TT-SCENES 3:38)
- `UIRequiresFullScreen` is still honored, but the app still resizes on open/close (TT-PREPARE).

## SDK behavior by build target (TT-PREPARE 0:30, spoken; DUO-PREPARE "Before you begin", written)

| App built with | Behavior on iPhone Duo |
|----------------|------------------------|
| Older than iOS 27 SDK | Runs. When closed, uses "the screen space to the left of the status bar and camera." When open, "a familiar size and aspect ratio." |
| iOS 27 SDK | "Your app will extend to the left of the status bar area on the inner display." |
| iOS 27.1 SDK | "Your app extends to the edge of the screen. Standard navigation and toolbar buttons now lay out vertically under the status bar." |

Apple's written version (DUO-PREPARE, 2026-10-26): iOS 26 SDK or earlier — "When the device is open, your app will appear in the center of the display, with empty space surrounding the window. When the device is closed, the outer display will show your app's content to the left of the status bar and camera." iOS 27 SDK — "resize to fill most of the inner display on iPhone Duo, but still avoid the status bar on the right edge." iOS 27.1 SDK or later — "extend to use the full display, with toolbars and tab bars appearing vertically below the status bar." Warning attached: "Before shipping an app build with the iOS 27.1 SDK, make sure your app's content adapts to the bars becoming vertical." There is **no opt-out**: "Once you link on iOS 27, you get iPhone resizability" (LAB-1).

Tooling: "download Xcode 27.1. Choose the iPhone Duo simulator to run your app in Device Hub. Use the control buttons at the bottom of the screen to open, close, rotate, or fold iPhone Duo." (TT-PREPARE). DUO-PREPARE lists three test tools: "The iOS resizable simulator in Device Hub", "iPhone Mirroring on macOS 27. Resize the window to extreme sizes in both directions", and "The iPhone Duo simulator in Xcode 27.1" — "the best way to test the full native experience." Apple's own `app-resizability` skill ships in Xcode 27.1 (`apple-app-resizability-skill.md`).

**Xcode 27.1 shipped.** Beta on 2026-09-18 (developer email). Evidence that it is no longer beta, as of 2026-10-26: the Xcode 27.1 Release Notes title and body contain no "beta"; DocC pages for the 27.1 APIs carry no beta flag; build 27A9275 with the iOS 27.1 SDK is installed locally; NEWS-1005 (October 5) tells developers to "Recompile existing apps with Xcode 27.1" and LAB-3 (mid-October) refers to developers already "using Xcode 27.1 and Device Hub". Apple did not publish a release date in any fetched page. **iPhone Duo reached customers on October 23, 2026 running iOS 27.1** (NEWS-1005; LAB-1). From the Xcode 27.1 Release Notes:
- "includes Swift 6.4 and SDKs for iOS 27.1, iPadOS 27, tvOS 27, watchOS 27, macOS 27, and visionOS 27... requires a Mac running macOS Tahoe 26.6 or later."
- Previews: "The canvas overrides picker now includes a Display group for previewing content on a device's alternative display."
- Simulator known issues: "Initial Simulator launch can take several minutes." "StandBy is unavailable in the iPhone Duo Simulator runtime." "Running and debugging most app extensions is unavailable in the iPhone Duo Simulator runtime."
- Mac Catalyst: "Projects that use APIs specific to iOS 27.1 show compile errors when building for Mac Catalyst... Workaround: Use build-time conditionals like `#if !targetEnvironment(macCatalyst)`."

PREP restates the SDK gate more simply: "Build your app with the latest version of Xcode to use all of the available screen space on iPhone Duo. When you build with Xcode 26 and earlier, your app doesn't extend under the status bar and camera." 

## Display specifications (APPLE-SPECS — apple.com tech specs, **not** developer documentation)

| Display | Diagonal | Pixel resolution | ppi |
|---------|----------|------------------|-----|
| Inner | 7.6-inch | 1878 × 2670 | 430 |
| Outer | 5.4-inch | 1398 × 2034 | 460 |

Both: ProMotion up to 120 Hz, Always-On, Dynamic Island, HDR, P3. Footnote: "When measured as a standard rectangular shape, the screens are 7.58 and 5.36 inches diagonally (actual viewable area is less)."

**Apple publishes a second, different inner-display figure.** App Store Connect's screenshot specification (ASC-SHOTS, fetched 2026-10-26) lists iPhone Duo screenshots as **1398 × 2034 px (outer display)** — matching the tech specs — and **2007 × 2853 px (inner display)**, which does not match 1878 × 2670; the app-preview page gives the same 2853 × 2007 "device resolution" with a "19.5:9 aspect ratio" note. Both pages are Apple's; neither explains the difference. Record both with their source; do not average, pick, or convert them.

Use these numbers only for marketing assets or bezel mockups. **Do not derive point sizes or a scale factor from them** (see next section), and do not put them into layout code — HIG-DUO says "Avoid fixed widths and display-specific dependencies."

## NOT documented by Apple as of 2026-10-26 — do not assume, ask the user or verify in the SDK

(Re-checked 2026-10-26 against the HIG page, the article, DUO-PREPARE, the news item, App Store Connect specs, the three Group Lab transcripts, and the released iOS 27.1 SDK. Every item below is still unpublished unless marked.)

- **Point (pt) dimensions** of either display, and the **display scale factor**. The HIG Layout page, as fetched on 2026-09-10, contains no iOS device-dimension table (earlier change-log entries refer to one). Dividing pixels by 3 is a guess; do not present it as fact — all the more so because Apple's two published inner-display pixel figures (1878 × 2670 vs 2007 × 2853) disagree.
- **Width in points of the vertical bar region** (the side strip holding Dynamic Island, status bar, toolbar, tab bar).
- **Hinge angle thresholds** for `.closed` / `.partiallyOpen` / `.fullyOpen`, and the angle range reported by `onHingeChange` / `UIHingeInteraction`.
- **Safe-area inset values** on either display, in any pose.
- **Inner-display aspect ratio in Apple's developer documentation.** The only published figure is the apple.com pixel resolution above (1878 × 2670, so not square). HIG says split arrangements decide horizontal vs vertical based on whether the *arrangement* is "wider than it is tall"; the arrangement's bounds, not the display's, are what matter, and they change with Split View, PiP, and folding.
- **Which of the "six device poses"** map to which size-class combinations beyond the outer/inner table above.
- **Any guidance for web, React Native, Flutter, Unity, or other non-native UI stacks.** Apple's material is SwiftUI, UIKit, AVFoundation/AVKit, and one HIG paragraph on games.
- **Device Hub pose-control details** beyond the one Tech Talk sentence; the Xcode 27.1 notes list only known issues (above).
- **Any way to detect which half of the device rests on a surface** in the seated pose, or **Core Motion axis mapping** per display (both asked in LAB-1; neither answered).
- **Any WebKit/Safari guidance for poses** (LAB-1 pointed only to "existing JavaScript standards around foldable devices").
- **Design kits:** LAB-1 said "Figma and Sketch design kits are available"; the Design Resources page on 2026-10-26 lists iPhone Duo only under Photoshop/PNG. Check the page before naming a format.
- **No longer open:** the SwiftUI hinge modifier is declared in the released SDK as `onHingeChange(isEnabled:_:)` with `DeviceHinge` / `DeviceHingeContext` (still no DocC page). All iOS 27.1 symbols lost their **beta** marker; see `api-index.md`.
