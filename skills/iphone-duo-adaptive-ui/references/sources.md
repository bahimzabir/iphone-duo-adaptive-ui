# Sources

Every fact in this skill traces to one of the Apple sources below. All were fetched on **2026-09-10**. Nothing in this skill comes from third-party blogs, forums, or inference. When a source and the live page disagree, the live page wins; re-fetch before relying on version-sensitive details.

## Primary (design guidance)

| ID | Title | URL | Notes |
|----|-------|-----|-------|
| HIG-DUO | Human Interface Guidelines — Designing for iPhone Duo | https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo | New page, change log dated September 9, 2026. Full text in `hig-designing-for-iphone-duo.md`. |
| HIG-LAYOUT | Human Interface Guidelines — Layout | https://developer.apple.com/design/human-interface-guidelines/layout | Size classes, safe areas, layout margins. Change log September 9, 2026: "Updated guidance to reflect current best practices." As fetched, the page has **no** iOS/iPadOS device-dimension table. |
| HIG-SPLIT | Human Interface Guidelines — Split views | https://developer.apple.com/design/human-interface-guidelines/split-views | General split-view guidance (no Duo-specific text as fetched). |
| HIG-TOOLBARS | Human Interface Guidelines — Toolbars | https://developer.apple.com/design/human-interface-guidelines/toolbars | General toolbar guidance (no Duo-specific text as fetched). |
| HIG-IOS | Human Interface Guidelines — Designing for iOS | https://developer.apple.com/design/human-interface-guidelines/designing-for-ios | HIG-DUO states these patterns still apply. |
| HIG-GAMES | Human Interface Guidelines — Designing for games | https://developer.apple.com/design/human-interface-guidelines/designing-for-games | Referenced by HIG-DUO for games. |

## Primary (developer guidance — Apple Tech Talks, September 2026)

These six talks are, as of 2026-09-10, the **only** published Apple source for the iOS 27.1 iPhone Duo APIs. Their DocC reference pages return 404. Chapter summaries and on-page sample code were extracted from the talk pages; spoken transcripts were read from the talks' English subtitle tracks.

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

## API pages probed on 2026-09-10

Every symbol in `api-index.md` was probed against the DocC JSON endpoint; that file's DocC column holds the per-symbol result. Summary:

- **Found (HTTP 200), beyond the table above:** `topBarPinnedTrailing`, `cancellationAction`, `pinnedTrailingGroup`, `leadingItemGroups`, `leftItemsSupplementBackButton`, `UITabBarController.sidebar`, SwiftUI `badge(_:)`, `UIBarButtonItem.visibilityPriority`, `NavigationStack`, `UINavigationController`, `safeAreaLayoutGuide`, `ignoresSafeArea`, `UIWindowScene.screen`, `displayScale`, `UIRequiresFullScreen`, `AVCaptureDevice.RotationCoordinator`, `AVCaptureDevice.dynamicAspectRatio`, `AVCapturePhotoOutput.isCameraSensorOrientationCompensationEnabled`.
- **Not found (HTTP 404):** `ArrangementView`, `arrangementViewStyle`, `overlayArrangementZIndex`, `GeometryProxy.reservedRegions`, `ReservedRegion`, `UIArrangementViewController`, `UISplitArrangement`, `UIView.reservedRegions`, `UIViewReservedRegion`, `toolbarVerticalBehavior`, `toolbarVerticalEdge`, `toolbarVerticalCompressionBehavior`, `verticalBarCompressionBehavior`, `verticalBarEdge`, `UIVerticalBarBehavior`, `preferredVerticalBarBehavior`, `axisBehavior` (both frameworks), `UIBarButtonItem.badge`, `onHingeChange`, `UIHingeInteraction`, `sceneAccessory`, `CameraCaptureAccessory`, `UIWindowSceneActivationAction`, `UICornerConfiguration`, `AVCaptureDeviceDirectionCoordinator`, `builtInOuterUltraWideCamera`, iOS 27.1 release notes, Xcode 27.1 release notes. A 404 can mean unpublished or a different path; treat every one as **pre-release, documented only by Tech Talk** until verified in the SDK.

## Secondary (official Apple, but not developer documentation)

| ID | Title | URL | Used for |
|----|-------|-----|----------|
| APPLE-SPECS | iPhone Duo — Tech Specs | https://www.apple.com/iphone-duo/specs/ | Display pixel resolution, diagonal size, ppi only. |
| DESIGN-RES | Apple Design Resources | https://developer.apple.com/design/resources/ | Lists a downloadable "iPhone Duo" bezel (Photoshop, PNG). No Figma/Sketch Duo template was listed at fetch time. |

## Apple documentation articles named in the talks (not fetched; titles as spoken)

- "Choosing a Camera by the Direction it Faces" (TT-CAMERA) — Apple Developer Documentation article on `AVCaptureDeviceDirectionCoordinator`.
- "Supporting Device Rotation in Your Camera App" (TT-CAMERA) — Apple Developer Documentation article on the rotation coordinator.
- "Modernize your UIKit app" (WWDC26 session, referenced by TT-PREPARE) — origin of the App Resizability skill and flexible-layout principles.
- "Support the Center Stage Front Camera in your iOS app" (WWDC26 session, referenced by TT-CAMERA).
- "What's new in SwiftUI" (WWDC26 session, referenced by TT-BARS for visibility priority).
