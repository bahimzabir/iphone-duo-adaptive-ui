# API index with provenance

Every symbol an agent might use for iPhone Duo work, with where Apple states it and its DocC reference status. First probed **2026-09-10** (all iOS 27.1 pages 404); re-probed **2026-09-22** after Xcode 27.1 beta shipped — nearly all now published and marked **iOS 27.1 beta**. "Code" means the exact spelling appears in an Apple on-page code sample; "spoken" means it appears only in narration or a chapter summary (spelling may be paraphrased — verify in the SDK). Minimum SDK is iOS 27.1 unless noted.

Legend — DocC status from HTTP probes of `https://developer.apple.com/tutorials/data/documentation/<path>.json` on 2026-09-22: ✅ HTTP 200 · ❌ HTTP 404 at every path tried (treat spelling as Tech-Talk-only) · "unverified" = not probed. "PREP" = the Apple article *Preparing your app for iPhone Duo*.

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
| `ToolbarItem(placement: .cancellationAction)` | SwiftUI | code (TT-BARS 5:00); PREP | ✅ | Custom back/close → top of vertical bar. |
| `ToolbarItem(placement: .topBarPinnedTrailing)` | SwiftUI | code (TT-BARS 5:24) | ✅ | Prominent action (Done). |
| `navigationItem.leftItemsSupplementBackButton = false` | UIKit | code (TT-BARS 5:00) | ✅ | Default is already `false`. |
| `navigationItem.leadingItemGroups` | UIKit | code (TT-BARS 5:00) | ✅ | |
| `navigationItem.pinnedTrailingGroup` | UIKit | code (TT-BARS 5:24) | ✅ | |
| `.axisBehavior(_:)` on toolbar content, `ToolbarItemAxisBehavior` `.automatic` / `.horizontalOnly` / `.verticalPreferred` | SwiftUI | code (TT-BARS 8:08, 8:36); PREP | ✅ | Doc: "If an item only supports horizontal bars and no horizontal bars are present, the item is not shown." `.verticalPreferred` wins when both bars exist. |
| `UIBarButtonItem.axisBehavior`, `UIBarButtonItem.AxisBehavior` `.automatic` / `.horizontalOnly` / `.verticalPreferred` | UIKit | code (TT-BARS 8:08); PREP | ✅ | Same semantics as SwiftUI. |
| `.badge(7)` on toolbar content | SwiftUI (iOS 26) | code (TT-BARS 9:27) | ✅ (`swiftui/view/badge(_:)`) | Turns text+symbol into symbol-only. |
| `UIBarButtonItem.badge = .count(7)` | UIKit (iOS 26) | code (TT-BARS 9:27) | ❌ (`uibarbuttonitem/badge` not found) | Apple says the badge API was "added in iOS 26"; path unknown. |
| `@Environment(\.toolbarVerticalEdge)` → `HorizontalEdge?` | SwiftUI | code (TT-BARS 10:36); PREP | ✅ | Read-only; "reflects the system's preferred edge... regardless of whether a vertical bar is currently visible"; `nil` where the system never places one. |
| `traitCollection.verticalBarEdge` → `UIVerticalBarEdge` | UIKit | code (TT-BARS 10:36); PREP | ✅ | `.unspecified` where the system never places a vertical bar. |
| `.toolbarVerticalCompressionBehavior(_:)`, `ToolbarVerticalCompressionBehavior` `.automatic` / `.prefersTabBar` / `.prefersToolbarItems` | SwiftUI | code (TT-BARS 12:23) | ✅ | Doc example: Files-style app prefers toolbar items. |
| `navigationItem.verticalBarCompressionBehavior`, `UIVerticalBarCompressionBehavior` `.automatic` / `.prefersBarItems` / `.prefersTabBar` | UIKit | code (TT-BARS 12:23) | ✅ | Default `.automatic`. |
| `ToolbarOverflowMenu { … }` | SwiftUI | code (TT-BARS 12:43); HIG-DUO link | ✅ | Always-in-overflow actions. |
| `navigationItem.additionalOverflowItems = UIDeferredMenuElement(…)` | UIKit | code (TT-BARS 12:43); HIG-DUO link | ✅ | |
| `.visibilityPriority(.high)` / `ToolbarItemVisibilityPriority` | SwiftUI | code (TT-BARS 13:21); HIG-DUO link | ✅ | `.automatic`, `.low`, `.high`, `init(lowerThan:)`, `init(higherThan:)`. |
| `UIBarButtonItem.visibilityPriority = .high` / `UIBarButtonItemVisibilityPriority` | UIKit | code (TT-BARS 13:21); HIG-DUO link | ✅ | `.standard`, `.low`, `.high`, custom inits. |
| `ToolbarItemGroup` / `UIBarButtonItemGroup` | SwiftUI / UIKit | HIG-DUO link | ✅ | Group instead of manual spacing. |
| `.toolbarVerticalBehavior(_:)`, `ToolbarVerticalBehavior` `.automatic` / `.disabled` | SwiftUI | code (TT-BARS 14:47); PREP | ✅ | Doc: "Treat it as a stable choice"; resolved by top view of `NavigationStack`, selected view of `TabView`, trailing-most column of `NavigationSplitView`. |
| `override var preferredVerticalBarBehavior: UIVerticalBarBehavior`, `.automatic` / `.disabled` | UIKit | code (TT-BARS 14:47); PREP | ✅ | Same guidance as SwiftUI. |
| `.presentationPlacement(_:)`, `PresentationPlacement` `.automatic` / `.center` / `.leading` / `.trailing` (SwiftUI, iOS 27.0); `UISheetPresentationController.preferredPlacement` (UIKit, iOS 27.0) | both | spoken (TT-BARS 3:09); PREP | ✅ | Inner display: centered/leading sheets get horizontal bars, trailing sheets get a vertical bar. "Only sheet presentations respect this placement." |
| `.backgroundExtensionEffect()` (SwiftUI, iOS 26); `UIBackgroundExtensionView` (UIKit, iOS 26) | both | PREP | ✅ | Extend a hero/background image under a vertical bar. |

## Reserved regions (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `proxy.reservedRegions(kind:options:layoutDirectionBehavior:)` on `GeometryProxy` → `[ReservedRegion]` | SwiftUI | code (TT-POSE 6:46); PREP | ✅ | Full signature: `reservedRegions(kind: ReservedRegion.Kind, options: ReservedRegion.QueryOptions = [], layoutDirectionBehavior: LayoutDirectionBehavior = .mirrors)`. Frames are mirrored for RTL by default; pass `.fixed` to keep hardware coordinates. |
| `ReservedRegion.QueryOptions.includeInactive` | SwiftUI | code (TT-POSE 7:22) | ✅ | Inactive fold region has zero width (TT-POSE). |
| `ReservedRegion.Kind` `.occlusion` / `.division` | SwiftUI | code (TT-POSE 8:07) | ✅ | Occlusion: "Dynamic Island, a camera, or window controls". Division: "the fold of a hinge". |
| `view.reservedRegions(kind:options:)` on `UIView` → `[UIView.ReservedRegion]` | UIKit | code (TT-POSE 7:03); PREP | ✅ | `UIView.ReservedRegion.Kind` `.occlusion` / `.division`; `QueryOptions.includeInactive`. Objective-C: `UIViewReservedRegion`, `reservedRegionsOfKind:options:`. |
| `ReservedRegion` / `UIView.ReservedRegion` properties: `frame` (includes margins), `margins`, `isActive`, `kind`, `id` | both | code (TT-POSE 7:03) | ✅ | `frame`: "The rect of the region in the view's coordinate space, including the margins." `margins`: "around the rect for interactive content." |
| Type names: `ReservedRegion` (SwiftUI), `UIView.ReservedRegion` (UIKit Swift), `UIViewReservedRegion` (UIKit Objective-C) | — | spoken (TT-PREPARE 8:08); PREP | ✅ | Confirmed. |

## Arrangement views (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `ArrangementView { primary } secondary: { secondary }` — `init(primary:secondary:)`, `init(_:)` from `ArrangementViewStyleConfiguration` | SwiftUI | code (TT-POSE 11:23); PREP | ✅ | Inside `NavigationStack`; PREP: "Avoid placing an arrangement view inside a navigation split view, list, scroll view, or other container that might cause part of your view to become inaccessible." |
| `.arrangementViewStyle(_:)`, `ArrangementViewStyle` `.automatic` / `.split` / `.overlay`; types `AutomaticArrangementViewStyle`, `SplitArrangementViewStyle`, `OverlayArrangementViewStyle`; custom styles via `makeBody(configuration:)` | SwiftUI | code (TT-POSE 12:00) | ✅ | "The default style is AutomaticArrangementViewStyle, which resolves to a split arrangement." |
| `SplitArrangementViewStyle.axes(_: Axis.Set)`; `OverlayArrangementViewStyle.axes(_:)` | SwiftUI | code (TT-POSE 12:41) | ✅ | Both styles support axis restriction. |
| `.arrangementViewStyle(.overlay)` | SwiftUI | code (TT-POSE 13:26) | ✅ | Doc example: player controls (primary) over video (secondary). |
| `@Environment(\.overlayArrangementZIndex)` → `Int` | SwiftUI | code (TT-POSE 14:07) | ✅ | "Views with a higher z-index are rendered on top of views with a lower z-index." |
| `UIArrangementViewController()` | UIKit | code (TT-POSE 11:26); PREP | ✅ | Root of a `UINavigationController`. |
| `setViewController(_:for:)` with `UIArrangementViewController.ViewPlacement` `.primary` / `.secondary` | UIKit | code (TT-POSE 11:26); PREP | ✅ | Objective-C: `setViewController:forPlacement:`. |
| `updateArrangement(_:animated:)` with `UISplitArrangement` (default) or `UIOverlayArrangement`; both have `axes(_:)`; `UISplitArrangement.Dimension` / `DimensionRange` (min/preferred/max) | UIKit | code (TT-POSE 13:07); PREP | ✅ | `animated` defaults to `false`. |
| `state(for:)` → `UIArrangementViewController.ViewState?` (`zIndex`) | UIKit | code (TT-POSE 14:21) | ✅ | |

## Hinge, scenes, accessories (iOS 27.1)

| Symbol | Framework | Evidence | DocC | Note |
|--------|-----------|----------|------|------|
| `.onHingeChange { previous, context in … }` | SwiftUI | code (TT-SCENES 1:44) | ❌ (still unpublished 2026-09-22) | `context.hinge` optional; `.status`, `.angle: Angle`. Only Tech-Talk source; UIKit equivalent is fully documented. |
| `UIHingeInteraction(updateHandler:)` → handler `(interaction, UIHingeInteraction.Update)`; `update.hinge: UIHinge?`; `UIHinge.angle: CGFloat` (radians), `UIHinge.status: UIHinge.Status` `.closed` / `.partiallyOpen` / `.fullyOpen` / `.unknown`; `isEnabled` | UIKit | spoken (TT-SCENES 0:49) | ✅ | `hinge == nil` when "the interaction has left a hierarchy that provides hinge updates." Doc: "don't depend on a particular update frequency or precision... prefer `status` over the angle." |
| `UIWindowScene.ActivationAction` (spoken as "UIWindowSceneActivationAction") | UIKit (iOS 15.0) | spoken (TT-SCENES 3:38) | ✅ | Existing API; "You can specify an alternate action to display on iPhone and apps that don't support multiple windows." |
| `.sceneAccessory(content:)`; `CameraCaptureAccessory(content:)` / `(isEnabled:content:)` | SwiftUI | code (TT-SCENES 5:43, 6:14); PREP | ✅ | Outer-display companion UI for camera apps. UIKit: `UISceneAccessory.cameraCapture(sceneConfiguration:userInfo:)` + `UIViewController.registerSceneAccessory(_:)` (iOS 27.0). |
| `.onAvailabilityChange { … }` on `SceneAccessoryContent` | SwiftUI | code (TT-SCENES 6:25) | ✅ (named in `sceneAccessory(content:)` doc) | |

## Camera (AVFoundation / AVKit, iOS 27.1)

| Symbol | Evidence | DocC | Note |
|--------|----------|------|------|
| `AVCaptureDevice.DeviceType.builtInOuterUltraWideCamera`, `.builtInInnerUltraWideCamera` | code (TT-CAMERA 4:06) | ❌ | New in iOS 27.1. |
| `AVCaptureDeviceDirectionCoordinator(view:deviceTypes:changeHandler:)`, `deviceDirections`, `forwardFacingDeviceDescriptors` (AVKit) | code (TT-CAMERA 4:06); PREP | ✅ | iOS 27.1 beta. Main-actor; one coordinator per display view; include rear cameras. Article: "Choosing a camera by the direction it faces". |
| `AVCaptureDevice.dynamicAspectRatio` | code (TT-CAMERA 7:51) | ✅ | Existing API. |
| `AVCapturePhotoOutput.isCameraSensorOrientationCompensationEnabled` | code (TT-CAMERA 8:34) | ✅ | Existing API. Set false after adopting rotation coordinator. |
| Rotation coordinator (`AVCaptureDevice.RotationCoordinator` on DocC) | spoken (TT-CAMERA 8:03) | ✅ (`avfoundation/avcapturedevice/rotationcoordinator`) | Existing API; "will update when your app moves displays." |

## Tooling

| Item | Evidence | Note |
|------|----------|------|
| Xcode 27.1 beta + iPhone Duo simulator in Device Hub (open / close / rotate / fold controls) | spoken (TT-PREPARE 1:17); Xcode 27.1 Beta Release Notes | Shipped by 2026-09-18. Notes: SDK iOS 27.1; requires macOS Tahoe 26.6+; Previews canvas gets a "Display" override group for the alternative display; known issues — first Simulator launch slow, StandBy and most app extensions unavailable in the iPhone Duo Simulator runtime; Mac Catalyst builds error on 27.1-only APIs (wrap in `#if !targetEnvironment(macCatalyst)`). |
| "App Resizability" skill in Xcode 27.1 (formerly the app modernization skill from "Modernize your UIKit app", WWDC26) | spoken (TT-PREPARE 9:12) | "It now supports SwiftUI and iPhone Duo." |
