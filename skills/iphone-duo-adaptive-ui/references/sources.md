# Sources

Every fact in this skill traces to one of the Apple sources below. First fetched **2026-09-10**; re-verified and extended **2026-09-22** after Apple shipped Xcode 27.1 beta and the API reference; re-verified and extended **2026-10-26** (three days after iPhone Duo went on sale) from the "Get ready for iPhone Duo" hub, its sub-pages, the Group Lab recordings, and the released iOS 27.1 SDK. Nothing in this skill comes from third-party blogs, forums, or inference. When a source and the live page disagree, the live page wins; re-fetch before relying on version-sensitive details.

## Primary (design guidance)

| ID | Title | URL | Notes |
|----|-------|-----|-------|
| HIG-DUO | Human Interface Guidelines — Designing for iPhone Duo | https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo | New page, change log dated September 9, 2026. Body text unchanged as of 2026-10-26: JSON `primaryContentSections` diffed against the local copy flagged 5 text nodes, all fragments split around inline links or differing only in quote characters; each was confirmed present by hand (e.g. "Use the … API to keep important elements clear of the center", "In task-oriented experiences, minimize the tab bar…"). Change log still ends at September 9, 2026. Full text in `hig-designing-for-iphone-duo.md`. |
| PREP | Technology Overviews — Preparing your app for iPhone Duo | https://developer.apple.com/documentation/technologyoverviews/preparing-your-app-for-iphone-duo | Published between 2026-09-10 and 2026-09-22. Body text unchanged as of 2026-10-26 (0 differences). Full text in `apple-preparing-your-app-for-iphone-duo.md`. |
| DUO-HUB | Get ready for iPhone Duo (landing page) | https://developer.apple.com/iphone-duo/ | Fetched 2026-10-26. Lists Tech Talks, three Group Labs, workshops, forum Q&As, Xcode 27.1 (no longer "beta"), HIG, design resources, PREP, DUO-PREPARE, App Store asset pages. Inventory in `apple-get-ready-prepare.md` §1. |
| DUO-PREPARE | Three steps to make your app shine on iPhone Duo | https://developer.apple.com/iphone-duo/prepare/ | New sub-page (stylesheet dated 2026-10-05). Full text in `apple-get-ready-prepare.md` §2: SDK-gate table, the `xcrun agent skills export` skill, pattern list (`UIScreen.main`, orientation, idiom, device sizes, full-bleed media), three test tools, App Store steps. |
| NEWS-1005 | News — Prepare and submit your apps for iPhone Duo (October 5, 2026) | https://developer.apple.com/news/?id=kkphp5qo | Availability date October 23, 2026; Xcode 27.1 release; App Store badge; "Starting April 2027, any apps or games submitted will need to include screenshots for iPhone Duo." Full text in `apple-get-ready-prepare.md` §3. |
| ASC-SHOTS | App Store Connect Help — Screenshot specifications | https://developer.apple.com/help/app-store-connect/reference/app-information/screenshot-specifications | iPhone Duo: 1398 × 2034 (outer), 2007 × 2853 (inner), both orientations; April 2027 requirement "for any app using the iOS 27.1 SDK or later". |
| ASC-PREVIEWS | App Store Connect Help — App preview specifications | https://developer.apple.com/help/app-store-connect/reference/app-information/app-preview-specifications | iPhone Duo device resolutions 2034 × 1398 and 2853 × 2007 ("19.5:9"); accepted 886 × 1920 / 1920 × 886. |
| HIG-LAYOUT | Human Interface Guidelines — Layout | https://developer.apple.com/design/human-interface-guidelines/layout | Size classes, safe areas, layout margins. Change log September 9, 2026: "Updated guidance to reflect current best practices." As fetched, the page has **no** iOS/iPadOS device-dimension table. |
| HIG-SPLIT | Human Interface Guidelines — Split views | https://developer.apple.com/design/human-interface-guidelines/split-views | General split-view guidance (no Duo-specific text as fetched). |
| HIG-TOOLBARS | Human Interface Guidelines — Toolbars | https://developer.apple.com/design/human-interface-guidelines/toolbars | General toolbar guidance (no Duo-specific text as fetched). |
| HIG-IOS | Human Interface Guidelines — Designing for iOS | https://developer.apple.com/design/human-interface-guidelines/designing-for-ios | HIG-DUO states these patterns still apply. |
| HIG-GAMES | Human Interface Guidelines — Designing for games | https://developer.apple.com/design/human-interface-guidelines/designing-for-games | Referenced by HIG-DUO for games. |

## Primary (developer guidance — Apple Tech Talks, September 2026)

On 2026-09-10 these six talks were the only published Apple source for the iOS 27.1 iPhone Duo APIs. As of 2026-09-22 the DocC reference pages exist (see below); the talks remain the source for design rationale and timestamps. Chapter summaries and on-page sample code were extracted from the talk pages; spoken transcripts were read from the talks' English subtitle tracks.

| ID | Title | URL |
|----|-------|-----|
| TT-DESIGN | Design for iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111466/ |
| TT-PREPARE | Prepare your app for iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111461/ |
| TT-BARS | Raise the bar with iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111462/ |
| TT-POSE | Strike a pose with adaptive layouts on iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111463/ |
| TT-SCENES | Leverage multiple displays and scenes on iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111464/ |
| TT-CAMERA | Build a great camera experience for iPhone Duo | https://developer.apple.com/videos/play/tech-talks/111465/ |

Landing page listing all six plus events: **Get Ready for iPhone Duo** — https://developer.apple.com/iphone-duo/ (on 2026-09-10 it said Xcode 27.1 beta and the article were "Coming later this month"; on 2026-10-26 both are linked as shipped — see DUO-HUB above).

## Primary (developer guidance — Apple Group Labs, Meet with Apple, September 2026)

Live Q&A panels of Apple engineers and designers. Days 1 and 2 were recorded the week after the Tech Talks (mid-September 2026, before Xcode 27.1 shipped); Day 3 opens with "Just ten days until iPhone Duo is in people's hands", so about October 13, 2026, after Xcode 27.1 was out. Read from the English subtitle tracks on 2026-10-26; transcribed Q&A in `group-lab-day1.md`, `group-lab-day2.md`, `group-lab-day3.md` (one file per session, question list at the top). Spoken, sometimes hedged — the files mark hedges and defer to written sources where they exist.

| ID | Title | URL | Status |
|----|-------|-----|--------|
| LAB-1 | iPhone Duo Group Lab — Day 1 | https://developer.apple.com/videos/play/meet-with-apple/285/ | Transcribed in full → `group-lab-day1.md`. |
| LAB-2 | iPhone Duo Group Lab — Day 2 | https://developer.apple.com/videos/play/meet-with-apple/286/ | Transcribed in full → `group-lab-day2.md`. |
| LAB-3 | iPhone Duo Group Lab — Day 3 | https://developer.apple.com/videos/play/meet-with-apple/287/ | Transcribed in full → `group-lab-day3.md`. |

## Primary (Apple's own agent skill, shipped in Xcode 27.1)

| ID | What | How obtained | Notes |
|----|------|--------------|-------|
| XC-SKILL | `app-resizability` skill (SKILL.md + 5 task files) | `xcrun agent skills export --output-dir <abs path>` on Xcode 27.1 (27A9275), 2026-10-26 | Named by DUO-PREPARE Step 1. Not vendored (no license file in the export); summarized with short quotes in `apple-app-resizability-skill.md`. |

## Primary (the released iOS 27.1 SDK itself)

| ID | What | Notes |
|----|------|-------|
| SDK-271 | iPhoneOS27.1.sdk inside Xcode 27.1 (27A9275) | Used on 2026-10-26 to verify spellings that still have no DocC page: `SwiftUICore.swiftinterface` (`onHingeChange`, `DeviceHinge`, `DeviceHingeContext`), `UIBarButtonItemBadge.h`, `UICornerConfiguration.h`, `UIVerticalBarEdge.h` (`systemTraitsAffectingVerticalBarEdge`), `UIHinge.h`, `AVCaptureDevice.h` (outer/inner ultra-wide device types). Rows marked 🧩 in `api-index.md`. |

## Primary (API reference pages that DO exist on DocC as of 2026-09-10)

| Symbol | URL |
|--------|-----|
| `ToolbarItemVisibilityPriority` (SwiftUI) | https://developer.apple.com/documentation/swiftui/toolbaritemvisibilitypriority |
| `visibilityPriority(_:)` (SwiftUI) | https://developer.apple.com/documentation/swiftui/toolbarcontent/visibilitypriority(_:) |
| `ToolbarOverflowMenu` (SwiftUI) | https://developer.apple.com/documentation/swiftui/toolbaroverflowmenu |
| `ToolbarItemGroup` (SwiftUI) | https://developer.apple.com/documentation/swiftui/toolbaritemgroup |
| `NavigationSplitView` (SwiftUI) | https://developer.apple.com/documentation/swiftui/navigationsplitview |
| `GeometryProxy` / `safeAreaInsets` (SwiftUI) | https://developer.apple.com/documentation/swiftui/geometryproxy/safeareainsets |
| `ConcentricRectangle` (SwiftUI) | https://developer.apple.com/documentation/swiftui/concentricrectangle |
| `defaultTabBarPlacement(_:)` (SwiftUI) | https://developer.apple.com/documentation/swiftui/view/defaulttabbarplacement(_:) |
| `UIBarButtonItemVisibilityPriority` (UIKit) | https://developer.apple.com/documentation/uikit/uibarbuttonitemvisibilitypriority |
| `UIBarButtonItemGroup` (UIKit) | https://developer.apple.com/documentation/uikit/uibarbuttonitemgroup |
| `UINavigationItem.additionalOverflowItems` (UIKit) | https://developer.apple.com/documentation/uikit/uinavigationitem/additionaloverflowitems |
| `UISplitViewController` (UIKit) | https://developer.apple.com/documentation/uikit/uisplitviewcontroller |
| `UIView.safeAreaInsets` (UIKit) | https://developer.apple.com/documentation/uikit/uiview/safeareainsets |
| Device Hub (Xcode) | https://developer.apple.com/documentation/xcode/device-hub |

## API reference pages (DocC) — status on 2026-09-22

All pages below returned HTTP 200 and carried **AVAILABILITY: iOS 27.1 beta, iPadOS 27.1 beta** on 2026-09-22; on 2026-10-26 the beta marker is gone (plain iOS 27.1) unless noted. `api-index.md` holds the per-symbol detail.

| Symbol | URL |
|--------|-----|
| `ArrangementView` (SwiftUI) | https://developer.apple.com/documentation/swiftui/arrangementview |
| `ArrangementViewStyle` (`.automatic`, `.split`, `.overlay`) | https://developer.apple.com/documentation/swiftui/arrangementviewstyle |
| `SplitArrangementViewStyle.axes(_:)` | https://developer.apple.com/documentation/swiftui/splitarrangementviewstyle/axes(_:) |
| `overlayArrangementZIndex` | https://developer.apple.com/documentation/swiftui/environmentvalues/overlayarrangementzindex |
| `ReservedRegion` (SwiftUI) | https://developer.apple.com/documentation/swiftui/reservedregion |
| `ReservedRegion.QueryOptions.includeInactive` | https://developer.apple.com/documentation/swiftui/reservedregion/queryoptions |
| `GeometryProxy.reservedRegions(kind:options:layoutDirectionBehavior:)` | https://developer.apple.com/documentation/swiftui/geometryproxy/reservedregions(kind:options:layoutdirectionbehavior:) |
| `toolbarVerticalBehavior(_:)` / `ToolbarVerticalBehavior` | https://developer.apple.com/documentation/swiftui/view/toolbarverticalbehavior(_:) |
| `toolbarVerticalEdge` | https://developer.apple.com/documentation/swiftui/environmentvalues/toolbarverticaledge |
| `toolbarVerticalCompressionBehavior(_:)` / `ToolbarVerticalCompressionBehavior` | https://developer.apple.com/documentation/swiftui/view/toolbarverticalcompressionbehavior(_:) |
| `axisBehavior(_:)` / `ToolbarItemAxisBehavior` | https://developer.apple.com/documentation/swiftui/toolbarcontent/axisbehavior(_:) |
| `presentationPlacement(_:)` / `PresentationPlacement` (iOS 27.0) | https://developer.apple.com/documentation/swiftui/view/presentationplacement(_:) |
| `backgroundExtensionEffect()` (iOS 26.0) | https://developer.apple.com/documentation/swiftui/view/backgroundextensioneffect() |
| `sceneAccessory(content:)` / `CameraCaptureAccessory` | https://developer.apple.com/documentation/swiftui/cameracaptureaccessory |
| `UIArrangementViewController` | https://developer.apple.com/documentation/uikit/uiarrangementviewcontroller |
| `UISplitArrangement` / `UIOverlayArrangement` | https://developer.apple.com/documentation/uikit/uisplitarrangement-swift.struct |
| `UIView.ReservedRegion` / `reservedRegions(kind:options:)` | https://developer.apple.com/documentation/uikit/uiview/reservedregion |
| `UIViewReservedRegion` (Objective-C) | https://developer.apple.com/documentation/uikit/uiviewreservedregion |
| `UIVerticalBarBehavior` / `preferredVerticalBarBehavior` | https://developer.apple.com/documentation/uikit/uiviewcontroller/preferredverticalbarbehavior |
| `UITraitCollection.verticalBarEdge` | https://developer.apple.com/documentation/uikit/uitraitcollection/verticalbaredge |
| `UINavigationItem.verticalBarCompressionBehavior` / `UIVerticalBarCompressionBehavior` | https://developer.apple.com/documentation/uikit/uinavigationitem/verticalbarcompressionbehavior |
| `UIBarButtonItem.axisBehavior` / `UIBarButtonItem.AxisBehavior` | https://developer.apple.com/documentation/uikit/uibarbuttonitem/axisbehavior-swift.property |
| `UISheetPresentationController.preferredPlacement` (iOS 27.0) | https://developer.apple.com/documentation/uikit/uisheetpresentationcontroller/preferredplacement |
| `UIBackgroundExtensionView` (iOS 26.0) | https://developer.apple.com/documentation/uikit/uibackgroundextensionview |
| `UIHinge` / `UIHingeInteraction` / `UIHingeInteraction.Update` | https://developer.apple.com/documentation/uikit/uihingeinteraction |
| `UISceneAccessory` (iOS 27.0) | https://developer.apple.com/documentation/uikit/uisceneaccessory |
| `UIWindowScene.ActivationAction` (iOS 15.0) | https://developer.apple.com/documentation/uikit/uiwindowscene/activationaction |
| `AVCaptureDeviceDirectionCoordinator` (AVKit) | https://developer.apple.com/documentation/avkit/avcapturedevicedirectioncoordinator |
| Article: Choosing a camera by the direction it faces | https://developer.apple.com/documentation/avkit/choosing-a-camera-by-the-direction-it-faces |
| Article: Registering a camera capture accessory on iPhone Duo | https://developer.apple.com/documentation/avfoundation/registering-a-camera-capture-accessory-on-iphone-duo |
| Article: Adapting your app when traits change (UIKit) | https://developer.apple.com/documentation/uikit/adapting-your-app-when-traits-change |
| Xcode 27.1 Release Notes (titled "Beta" on 2026-09-22; "beta" absent from title and body on 2026-10-26) | https://developer.apple.com/documentation/xcode-release-notes/xcode-27_1-release-notes |
| Running your app on simulated or physical devices (Device Hub) | https://developer.apple.com/documentation/xcode/running-your-app-on-simulated-or-physical-devices |

**Still no DocC page on 2026-10-26 (HTTP 404 at every path tried):** the SwiftUI hinge modifier (`onHingeChange`), `UIBarButtonItem.badge`, `UICornerConfiguration`, the outer/inner ultra-wide `AVCaptureDevice.DeviceType` pages, and iOS/iPadOS 27.1 release notes. All but the release notes are now **SDK-verified** (SDK-271 above). On 2026-10-26 the DocC pages that do exist no longer carry the "beta" marker (e.g. `ArrangementView`, `UIHingeInteraction`: platforms iOS 27.1, no beta flag), and the Xcode 27.1 Release Notes body contains no occurrence of "beta".

**Not fetched (blocked):** the Apple Developer Forums Q&A threads linked from DUO-HUB — Group Lab highlights https://developer.apple.com/forums/thread/847644 (via https://apple.co/iPhoneDuo-QA01), Photos & Camera Q&A https://developer.apple.com/forums/activities/1659080, SwiftUI Q&A https://developer.apple.com/forums/activities/1664080, UIKit Q&A https://developer.apple.com/forums/activities/1670080. Each redirects to a human-verification page for automated clients. Open them in a browser if a question is not answered here.

## Secondary (official Apple, but not developer documentation)

| ID | Title | URL | Used for |
|----|-------|-----|----------|
| APPLE-SPECS | iPhone Duo — Tech Specs | https://www.apple.com/iphone-duo/specs/ | Display pixel resolution, diagonal size, ppi only. |
| APPLE-MAIL | Apple Developer email "Start building for iPhone Duo." (2026-09-18) | — | States "Get Xcode 27.1 beta and new design kits to start creating for iPhone Duo." Used only to date the Xcode 27.1 beta release. |
| DESIGN-RES | Apple Design Resources | https://developer.apple.com/design/resources/ | Lists a downloadable "iPhone Duo" bezel (Photoshop, PNG) on 2026-09-10 and still on 2026-10-26. LAB-1 said "Figma and Sketch design kits are available"; the page does not name a Duo-specific Figma/Sketch file. |

## Apple documentation articles named in the talks (not fetched; titles as spoken)

- "Choosing a Camera by the Direction it Faces" (TT-CAMERA) — Apple Developer Documentation article on `AVCaptureDeviceDirectionCoordinator`.
- "Supporting Device Rotation in Your Camera App" (TT-CAMERA) — Apple Developer Documentation article on the rotation coordinator.
- "Modernize your UIKit app" (WWDC26 session, referenced by TT-PREPARE) — origin of the App Resizability skill and flexible-layout principles.
- "Support the Center Stage Front Camera in your iOS app" (WWDC26 session, referenced by TT-CAMERA).
- "What's new in SwiftUI" (WWDC26 session, referenced by TT-BARS for visibility priority).
