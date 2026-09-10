# API index with provenance

Every symbol an agent might use for iPhone Duo work, with where Apple states it and whether a DocC reference page existed on **2026-09-10**. "Code" means the exact spelling appears in an Apple on-page code sample; "spoken" means it appears only in narration or a chapter summary (spelling may be paraphrased — verify in the SDK). Minimum SDK is iOS 27.1 unless noted.

Legend — DocC status comes from HTTP probes of `https://developer.apple.com/tutorials/data/documentation/<path>.json` on 2026-09-10: ✅ HTTP 200 · ❌ HTTP 404 at the probed path (unpublished, or published under a different path — treat as unverified) · "unverified" = not probed.

## Size classes, safe areas, screens (existing APIs, re-emphasized)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `@Environment(\.horizontalSizeClass)`, `\.verticalSizeClass` | SwiftUI | code (TT-PREPARE 2:59) | ✅ | Basis for all layout decisions. |
| `traitCollection.horizontalSizeClass` / `.verticalSizeClass` | UIKit | code (TT-PREPARE 2:59) | ✅ | |
| `GeometryProxy.safeAreaInsets` | SwiftUI | HIG-DUO link | ✅ | |
| `UIView.safeAreaInsets`, `safeAreaLayoutGuide` | UIKit | code (TT-PREPARE 6:52, 7:30) | ✅ | Handle each edge independently. |
| `.ignoresSafeArea()` | SwiftUI | code (TT-PREPARE 7:07) | ✅ | Background art only. |
| `window?.windowScene?.screen` | UIKit | code (TT-PREPARE 4:16) | ✅ | Replaces `UIScreen.main`, which "will be deprecated." |
| `traitCollection.displayScale` | UIKit | code (TT-PREPARE 9:25) | ✅ | Replaces `UIScreen.main.scale`. |
| `ConcentricRectangle` | SwiftUI (iOS 26) | code (TT-PREPARE 4:30) | ✅ | "updated to work with the screen shapes on iPhone Duo." |
| `UICornerConfiguration` | UIKit (iOS 26) | spoken + code comment (TT-PREPARE 4:30) | ❌ | Named by Apple; not found at `uikit/uicornerconfiguration`. |
| `UIRequiresFullScreen` (Info.plist) | UIKit | spoken (TT-PREPARE) | ✅ | Honored, but app still resizes on open/close. |

## Navigation containers

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `NavigationSplitView` | SwiftUI | HIG-DUO link; TT-PREPARE | ✅ | Collapses closed, tiles/overlays open; adapts to fold. |
| `UISplitViewController` | UIKit | HIG-DUO link; TT-PREPARE | ✅ | Same. |
| `NavigationStack` | SwiftUI | code (TT-BARS 2:24, TT-POSE 11:23) | ✅ | Required host for `.toolbar` to go vertical. |
| `TabView` / `.defaultTabBarPlacement(.sidebar)` | SwiftUI | code (TT-PREPARE 5:44) | ✅ | Sidebar on inner display (optional). |
| `UITabBarController` / `tabBarController.sidebar.preferredPlacement = .sidebar` | UIKit | code (TT-PREPARE 5:44) | ✅ (`uitabbarcontroller/sidebar-swift.property`) | |
| `UINavigationController` | UIKit | spoken (TT-BARS 2:00) | ✅ | Prefer over custom `UINavigationBar`/`UIToolbar`. |

## Vertical bars and toolbar items (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `ToolbarItem(placement: .cancellationAction)` | SwiftUI | code (TT-BARS 5:00) | ✅ (placement type) | Custom back/close → top of vertical bar. |
| `ToolbarItem(placement: .topBarPinnedTrailing)` | SwiftUI | code (TT-BARS 5:24) | ✅ | Prominent action (Done). |
| `navigationItem.leftItemsSupplementBackButton = false` | UIKit | code (TT-BARS 5:00) | ✅ | Default is already `false`. |
| `navigationItem.leadingItemGroups` | UIKit | code (TT-BARS 5:00) | ✅ | |
| `navigationItem.pinnedTrailingGroup` | UIKit | code (TT-BARS 5:24) | ✅ | |
| `.axisBehavior(.verticalPreferred)` / `.axisBehavior(.horizontalOnly)` on `ToolbarItem` | SwiftUI | code (TT-BARS 8:08, 8:36) | ❌ | Spoken name "AxisBehavior API". |
| `UIBarButtonItem.axisBehavior = .verticalPreferred` / `.horizontalOnly` | UIKit | code (TT-BARS 8:08) | ❌ | |
| `.badge(7)` on toolbar content | SwiftUI (iOS 26) | code (TT-BARS 9:27) | ✅ (`swiftui/view/badge(_:)`) | Turns text+symbol into symbol-only. |
| `UIBarButtonItem.badge = .count(7)` | UIKit (iOS 26) | code (TT-BARS 9:27) | ❌ (`uibarbuttonitem/badge` not found) | Apple says the badge API was "added in iOS 26"; path unknown. |
| `@Environment(\.toolbarVerticalEdge)` | SwiftUI | code (TT-BARS 10:36) | ❌ | Populated when items can be vertical; nil otherwise. |
| `traitCollection.verticalBarEdge` | UIKit | code (TT-BARS 10:36) | ❌ | |
| `.toolbarVerticalCompressionBehavior(.prefersToolbarItems)` | SwiftUI | code (TT-BARS 12:23) | ❌ | Spoken as "toolbarCompressionBehavior API". Default prefers tab bar. |
| `navigationItem.verticalBarCompressionBehavior = .prefersBarItems` | UIKit | code (TT-BARS 12:23) | ❌ | |
| `ToolbarOverflowMenu { … }` | SwiftUI | code (TT-BARS 12:43); HIG-DUO link | ✅ | Always-in-overflow actions. |
| `navigationItem.additionalOverflowItems = UIDeferredMenuElement(…)` | UIKit | code (TT-BARS 12:43); HIG-DUO link | ✅ | |
| `.visibilityPriority(.high)` / `ToolbarItemVisibilityPriority` | SwiftUI | code (TT-BARS 13:21); HIG-DUO link | ✅ | `.automatic`, `.low`, `.high`, `init(lowerThan:)`, `init(higherThan:)`. |
| `UIBarButtonItem.visibilityPriority = .high` / `UIBarButtonItemVisibilityPriority` | UIKit | code (TT-BARS 13:21); HIG-DUO link | ✅ | `.standard`, `.low`, `.high`, custom inits. |
| `ToolbarItemGroup` / `UIBarButtonItemGroup` | SwiftUI / UIKit | HIG-DUO link | ✅ | Group instead of manual spacing. |
| `.toolbarVerticalBehavior(.disabled)` | SwiftUI | code (TT-BARS 14:47) | ❌ | Opt out of vertical bar. |
| `override var preferredVerticalBarBehavior: UIVerticalBarBehavior { .disabled }` | UIKit | code (TT-BARS 14:47) | ❌ | Spoken as "preferredVerticalBarBehavior APIs". |
| Sheet `preferredPlacement` (left/right on inner display) | — | spoken (TT-BARS 3:09) | unverified (name as spoken, not probed) | Right-placed sheets get a vertical bar; left-placed do not. |

## Reserved regions (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `proxy.reservedRegions(kind: .division)` on `GeometryProxy` | SwiftUI | code (TT-POSE 6:46) | ❌ | Also via `onGeometryChange`. |
| `proxy.reservedRegions(kind: .division, options: .includeInactive)` | SwiftUI | code (TT-POSE 7:22) | ❌ | Inactive fold region has zero width. |
| `proxy.reservedRegions(kind: .occlusion)` | SwiftUI | code (TT-POSE 8:07) | ❌ | Camera. |
| `view.reservedRegions(kind: .division)` on `UIView` | UIKit | code (TT-POSE 7:03) | ❌ | |
| `region.frame` | both | code (TT-POSE 7:03) | ❌ | |
| Type names `ReservedRegion` (SwiftUI), `UIViewReservedRegion` (UIKit) | — | spoken (TT-PREPARE 8:08) | ❌ | Not seen in code; verify. |

## Arrangement views (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `ArrangementView { primary } secondary: { secondary }` | SwiftUI | code (TT-POSE 11:23) | ❌ | Inside `NavigationStack`; not inside `List`/`ScrollView`. |
| `.arrangementViewStyle(.split)` | SwiftUI | code (TT-POSE 12:00) | ❌ | Default style. |
| `.arrangementViewStyle(.split.axes(.horizontal))` | SwiftUI | code (TT-POSE 12:41) | ❌ | Spoken: "the axes method on the split ArrangementStyle". |
| `.arrangementViewStyle(.overlay)` | SwiftUI | code (TT-POSE 13:26) | ❌ | |
| `@Environment(\.overlayArrangementZIndex)` → `Int` | SwiftUI | code (TT-POSE 14:07) | ❌ | `> 0` means the view is on top. |
| `UIArrangementViewController()` | UIKit | code (TT-POSE 11:26) | ❌ | Root of a `UINavigationController`. |
| `setViewController(_:for: .primary / .secondary)` | UIKit | code (TT-POSE 11:26) | ❌ | |
| `updateArrangement(.split.axes(.horizontal))` | UIKit | code (TT-POSE 13:07) | ❌ | Spoken: "UISplitArrangement type". |
| `state(for: .primary)?.zIndex` | UIKit | code (TT-POSE 14:21) | ❌ | Spoken: "state for view placement method". |

## Hinge, scenes, accessories (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `.onHingeChange { previous, context in … }` | SwiftUI | code (TT-SCENES 1:44) | ❌ | `context.hinge` is optional; `.status` ∈ closed / partiallyOpen / fullyOpen; `.angle: Angle`. |
| `UIHingeInteraction` | UIKit | spoken (TT-SCENES 0:49) | ❌ | |
| `UIWindowSceneActivationAction` | UIKit | spoken (TT-SCENES 3:38) | ❌ | Hides itself when new windows unavailable. |
| `.sceneAccessory { CameraCaptureAccessory(isEnabled:) { … } }` | SwiftUI | code (TT-SCENES 5:43, 6:14) | ❌ | Outer-display companion UI for camera apps. |
| `.onAvailabilityChange { … }` on the accessory | SwiftUI | code (TT-SCENES 6:25) | ❌ | |

## Camera (AVFoundation / AVKit, iOS 27.1)

| Symbol | Evidence | DocC | Note |
|--------|----------|------|------|
| `AVCaptureDevice.DeviceType.builtInOuterUltraWideCamera`, `.builtInInnerUltraWideCamera` | code (TT-CAMERA 4:06) | ❌ | New in iOS 27.1. |
| `AVCaptureDeviceDirectionCoordinator(view:deviceTypes:changeHandler:)` (AVKit) | code (TT-CAMERA 4:06) | ❌ | New in iOS 27.1. Main-actor; handler gets `AVCaptureDeviceDescriptor`s. Apple names a DocC article "Choosing a Camera by the Direction it Faces". |
| `AVCaptureDevice.dynamicAspectRatio` | code (TT-CAMERA 7:51) | ✅ | Existing API. |
| `AVCapturePhotoOutput.isCameraSensorOrientationCompensationEnabled` | code (TT-CAMERA 8:34) | ✅ | Existing API. Set false after adopting rotation coordinator. |
| Rotation coordinator (`AVCaptureDevice.RotationCoordinator` on DocC) | spoken (TT-CAMERA 8:03) | ✅ (`avfoundation/avcapturedevice/rotationcoordinator`) | Existing API; "will update when your app moves displays." |

## Tooling

| Item | Evidence | Note |
|------|----------|------|
| Xcode 27.1 + iPhone Duo simulator in Device Hub (open / close / rotate / fold controls) | spoken (TT-PREPARE 1:17) | Xcode 27.1 beta "Coming later this month" on 2026-09-10. |
| "App Resizability" skill in Xcode 27.1 (formerly the app modernization skill from "Modernize your UIKit app", WWDC26) | spoken (TT-PREPARE 9:12) | "It now supports SwiftUI and iPhone Duo." |
