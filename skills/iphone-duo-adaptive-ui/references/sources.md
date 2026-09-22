# Sources

Every fact in this skill traces to one of the Apple sources below. First fetched **2026-09-10**; re-verified and extended **2026-09-22** after Apple shipped Xcode 27.1 beta and the API reference. Nothing in this skill comes from third-party blogs, forums, or inference. When a source and the live page disagree, the live page wins; re-fetch before relying on version-sensitive details.

## Primary (design guidance)

| ID | Title | URL | Notes |
|----|-------|-----|-------|
| HIG-DUO | Human Interface Guidelines — Designing for iPhone Duo | https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo | New page, change log dated September 9, 2026. Unchanged as of 2026-09-22. Full text in `hig-designing-for-iphone-duo.md`. |
| PREP | Technology Overviews — Preparing your app for iPhone Duo | https://developer.apple.com/documentation/technologyoverviews/preparing-your-app-for-iphone-duo | Published between 2026-09-10 and 2026-09-22. Full text in `apple-preparing-your-app-for-iphone-duo.md`. |
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

Landing page listing all six plus events: **Get Ready for iPhone Duo** — https://developer.apple.com/iphone-duo/ (states Xcode 27.1 beta and the documentation article "Preparing your app for iPhone Duo" are "Coming later this month" as of the fetch date).

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

All pages below returned HTTP 200 and carry **AVAILABILITY: iOS 27.1 beta, iPadOS 27.1 beta** unless noted. `api-index.md` holds the per-symbol detail.

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
| Xcode 27.1 Beta Release Notes | https://developer.apple.com/documentation/xcode-release-notes/xcode-27_1-release-notes |
| Running your app on simulated or physical devices (Device Hub) | https://developer.apple.com/documentation/xcode/running-your-app-on-simulated-or-physical-devices |

**Still not found on 2026-09-22 (HTTP 404 at every path tried):** the SwiftUI hinge modifier (`onHingeChange` — named in TT-SCENES and its sample code; Apple's site search was also checked), `UIBarButtonItem.badge`, `UICornerConfiguration`, `AVCaptureDevice.DeviceType.builtInOuterUltraWideCamera` page, iOS 27.1 release notes. Treat those spellings as Tech-Talk-only.

## Secondary (official Apple, but not developer documentation)

| ID | Title | URL | Used for |
|----|-------|-----|----------|
| APPLE-SPECS | iPhone Duo — Tech Specs | https://www.apple.com/iphone-duo/specs/ | Display pixel resolution, diagonal size, ppi only. |
| APPLE-MAIL | Apple Developer email "Start building for iPhone Duo." (2026-09-18) | — | States "Get Xcode 27.1 beta and new design kits to start creating for iPhone Duo." Used only to date the Xcode 27.1 beta release. |
| DESIGN-RES | Apple Design Resources | https://developer.apple.com/design/resources/ | Lists a downloadable "iPhone Duo" bezel (Photoshop, PNG). No Figma/Sketch Duo template was listed at fetch time. |

## Apple documentation articles named in the talks (not fetched; titles as spoken)

- "Choosing a Camera by the Direction it Faces" (TT-CAMERA) — Apple Developer Documentation article on `AVCaptureDeviceDirectionCoordinator`.
- "Supporting Device Rotation in Your Camera App" (TT-CAMERA) — Apple Developer Documentation article on the rotation coordinator.
- "Modernize your UIKit app" (WWDC26 session, referenced by TT-PREPARE) — origin of the App Resizability skill and flexible-layout principles.
- "Support the Center Stage Front Camera in your iOS app" (WWDC26 session, referenced by TT-CAMERA).
- "What's new in SwiftUI" (WWDC26 session, referenced by TT-BARS for visibility priority).
