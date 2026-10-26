# iPhone Duo readiness audit checklist

Use this to review an existing SwiftUI or UIKit app, or to self-check generated UI. Every line derives from an Apple statement (source tag in brackets; IDs in `sources.md`). Mark each item **pass / fail / n/a** and record the file or view concerned.

## A. Build and test setup

- [ ] App is built with the iOS 27.1 SDK (older SDKs do not get edge-to-edge layout or vertical bars). [TT-PREPARE 0:30]
- [ ] App has been run in the iPhone Duo simulator via Device Hub in every pose the controls offer: open, closed, rotated, folded. [TT-PREPARE 1:17]
- [ ] App has been tested in Split View multitasking on both the left and right side (vertical bar may be on either edge). [TT-PREPARE, "Respect safe areas" chapter, Device Hub drag test]
- [ ] Apple's `app-resizability` skill has been run (Xcode coding assistant, or exported with `xcrun agent skills export --output-dir <abs path>`); its findings fixed or explicitly deferred. [DUO-PREPARE Step 1; `apple-app-resizability-skill.md`]
- [ ] Info.plist prerequisites from Apple's skill checked read-only: a launch screen key is present (else `ITMS-90870`); iPad orientation declaration lists all four or none; `UIRequiresFullScreen` noted but not deleted. [XC-SKILL]
- [ ] Tested in all three of Apple's tools: iOS resizable simulator in Device Hub; iPhone Mirroring on macOS 27 "to extreme sizes in both directions"; iPhone Duo simulator in Xcode 27.1. [DUO-PREPARE Step 3]
- [ ] Tested with Reduce Transparency on: the system draws an opaque rectangle behind the vertical bar; content next to it still reads. [LAB-1]

## A2. Apple's own walkthrough (PREP "Address common layout and resizing considerations")

- [ ] Checked "how your app appears on both displays, when closed, open, or partially folded" and rotated in each pose.
- [ ] "Confirm your views resize well in each supported orientation and pose."
- [ ] "Inspect how the system presents your app's navigation bars, toolbars, and tab bars vertically on the side of the display."
- [ ] "Identify any views, sheets, or popovers that position awkwardly when you fold or open iPhone Duo."
- [ ] "Identify elements or controls in your views that appear in the fold, and are difficult to see or interact with."
- [ ] "Size your views relative to their container rather than to fixed iPhone dimensions." / "Make layout calculations based on your scene or containing view's bounds rather than screen dimensions."
- [ ] UIKit: Auto Layout adopted; size-class changes observed via automatic trait tracking. [PREP; "Adapting your app when traits change"]

## B. Layout decisions

- [ ] No layout decision keys off `userInterfaceIdiom`, interface orientation, or device model. Size classes only. [HIG-LAYOUT; TT-PREPARE 2:46]
- [ ] No fixed widths, breakpoints, or metrics tied to a specific screen. [HIG-DUO; TT-DESIGN 3:42]
- [ ] No `UIScreen.main` (or any static screen reference). Uses environment / trait collection / scene bounds, or `view.window?.windowScene?.screen`; no stored `UIScreen`; windows created with `UIWindow(windowScene:)`, never `UIWindow(frame: UIScreen.main.bounds)`. [TT-PREPARE 3:57; DUO-PREPARE]
- [ ] Size is re-read whenever it changes: size-dependent layout lives in `layoutSubviews` / `viewDidLayoutSubviews` (or `GeometryReader` / `onGeometryChange`), with `viewWillTransition(to:with:)` for size-change work; nothing sized once at launch or in `viewIsAppearing`. [DUO-PREPARE]
- [ ] `traitCollection.displayScale` / `@Environment(\.displayScale)` used instead of `UIScreen.main.scale`. [TT-PREPARE 9:25; DUO-PREPARE]
- [ ] No `UIDevice.current.orientation`, `statusBarOrientation`, `interfaceOrientation`, or `windowScene.effectiveGeometry.interfaceOrientation` in layout logic; aspect decided by comparing the width and height of `view.bounds` / `superview.bounds` / `GeometryReader` size. [DUO-PREPARE; XC-SKILL]
- [ ] No `userInterfaceIdiom == .phone` / `.pad` layout branches; no code that treats `horizontalSizeClass == .regular` as "iPad"; no `bounds.height == 844`-style device-size checks or known-size lists. [DUO-PREPARE]
- [ ] Full-bleed hero images/video (`.scaleAspectFill`, `.aspectRatio(contentMode: .fill)`) were checked on the wider display; fill vs fit chosen by size class / aspect ratio, or a focal point set. [DUO-PREPARE]
- [ ] Both size-class layouts exist: compact-width (outer display) and regular-width (inner display). The regular-width layout is not a stretched compact one. [HIG-DUO; TT-DESIGN 7:34]
- [ ] Functionality and element state are identical across displays and poses; the inner display may add a hierarchy level but never removes one. [HIG-DUO "Create a consistent experience"]
- [ ] Screen-corner-hugging UI uses `ConcentricRectangle` / `UICornerConfiguration`. [TT-PREPARE 4:30]

## C. Safe areas and margins

- [ ] Interactive/foreground content is inside the safe area (SwiftUI default; UIKit `safeAreaLayoutGuide` or `safeAreaInsets`). [TT-PREPARE 6:06]
- [ ] Only background/full-bleed art uses `ignoresSafeArea()` / `view.bounds`. [TT-PREPARE 7:07]
- [ ] No code assumes left/right (or top/bottom) insets are equal; each edge handled independently; no inset-threshold tests; no hardware constants subtracted from insets. [TT-PREPARE 7:30; XC-SKILL safe-area §6]
- [ ] Direction vs physical edge is deliberate: `safeAreaLayoutGuide.leading/trailingAnchor` when the code means a direction; `safeAreaInsets.left/.right` only for physical edges; custom bar placement reads `traitCollection.verticalBarEdge` with `systemTraitsAffectingVerticalBarEdge` registration outside layout methods. [XC-SKILL]
- [ ] Layout margins are used and treated as asymmetric. [TT-PREPARE]
- [ ] In full-width immersive layouts, nothing interactive can sit under the vertical bar, Dynamic Island, or status bar. [HIG-DUO; TT-DESIGN 6:33]

## D. Navigation containers

- [ ] Hierarchical content uses `NavigationSplitView` / `UISplitViewController`; it collapses closed and expands open. [HIG-DUO "Split views"; TT-PREPARE 5:01]
- [ ] Tabs use `TabView` / `UITabBarController`. If information-dense, sidebar placement on the inner display was considered. [TT-PREPARE 5:44; TT-DESIGN]
- [ ] Sheets, popovers, context menus, alerts are system presentations (they get fold avoidance for free). [TT-DESIGN 9:28; TT-PREPARE]
- [ ] No custom `UIToolbar`, `UINavigationBar`, or `UITabBar` used as the app's primary bars (their items are not considered for vertical layout). [TT-BARS 2:00]

## E. Vertical bars and toolbar items

- [ ] `.toolbar` is applied inside `NavigationStack` / `NavigationSplitView`; UIKit items are on `navigationItem` inside `UINavigationController`. [TT-BARS 2:00]
- [ ] Every non-text toolbar item has both a title and a symbol (`Label` / `UIBarButtonItem` title + image). [HIG-DUO; TT-BARS 5:56]
- [ ] Text-only items are minimized; inline counts became badges. [HIG-DUO; TT-BARS 9:00]
- [ ] Items that swap between symbol and text (custom Select/Done) use `.axisBehavior(.horizontalOnly)`. [TT-BARS 8:00]
- [ ] Custom views that support a vertical layout opt in with `.axisBehavior(.verticalPreferred)` and fit the bar's fixed width. [TT-BARS 8:00, 10:07]
- [ ] Custom bar views read `toolbarVerticalEdge` / `verticalBarEdge` if they need to adapt, and stay legible with Reduce Transparency on. [TT-BARS 10:07]
- [ ] Hero/background images extend under the vertical bar with `backgroundExtensionEffect()` / `UIBackgroundExtensionView`. [PREP]
- [ ] Sheets on the inner display use `presentationPlacement` / `preferredPlacement` deliberately (trailing = vertical bar; centered/leading = horizontal). [PREP]
- [ ] Custom back/close uses `.cancellationAction` / leading item with `leftItemsSupplementBackButton = false`; prominent action uses `.topBarPinnedTrailing` / `pinnedTrailingGroup`. [TT-BARS 4:29]
- [ ] Items are grouped with `ToolbarItemGroup` / `UIBarButtonItemGroup`; no manual spacers. [HIG-DUO]
- [ ] Any app-specific overflow menu was folded into `ToolbarOverflowMenu` / `additionalOverflowItems`; the ellipsis is used only for overflow. [HIG-DUO; TT-BARS 11:40]
- [ ] Visibility priorities assigned (groups first, then items) so frequent actions and badged items overflow last. [HIG-DUO; TT-BARS 13:10]
- [ ] Compression preference set per view: default (toolbar compresses first) for navigation-focused views; `.prefersToolbarItems` / `.prefersBarItems` for task-oriented views. [HIG-DUO; TT-BARS 11:40]
- [ ] Vertical bar disabled only where Apple's exceptions apply (single-page bottom-heavy layout like Calculator; single-control sheet). [HIG-DUO; TT-BARS 14:21]
- [ ] Keyboard accessory bars stay attached to the keyboard. [TT-BARS]
- [ ] Controls that belong to a non-edge pane (e.g. list actions in a split view) stay with that pane, not moved to the side. [HIG-DUO]

## F. Fold handling (inner display)

- [ ] Centered layouts were audited: converted to two columns or given a displacement rule. [TT-POSE 16:34]
- [ ] Grids prefer an even number of columns; spacing grows around the hinge while outer margins are preserved. [HIG-DUO; TT-POSE 5:12]
- [ ] Two-view side-by-side or layered layouts (`HStack`/`VStack`/`ZStack`-style) use `ArrangementView` / `UIArrangementViewController` with the matching style. [HIG-DUO "Arrangement views"; TT-POSE 14:39]
- [ ] Arrangement views contain no navigation containers and are not inside `NavigationSplitView`, `List`, `ScrollView`, "or other container that might cause part of your view to become inaccessible." [HIG-DUO; TT-POSE 16:09; PREP]
- [ ] Highest-priority manually laid-out controls query `reservedRegions(kind: .division)` and displace themselves; RTL handled (default mirroring or `layoutDirectionBehavior: .fixed`). [TT-POSE 16:34; DocC]
- [ ] Camera-centric UI queries `reservedRegions(kind: .occlusion)` and keeps content clear when active. [TT-POSE 0:27, 8:07]
- [ ] Displacement is minimal: only what must move, moved together when related; scrolling content does not displace. [HIG-DUO; TT-POSE 2:26]

## G. Hinge, scenes, multitasking

- [ ] Hinge angle (`onHingeChange` / `UIHingeInteraction`) is used only for effects/interactions, never for layout; nil hinge handled. [TT-SCENES 1:18, 2:35]
- [ ] Multi-scene apps handle scene-request errors and use `UIWindowSceneActivationAction` (new windows only on the inner display). [TT-SCENES 3:38]
- [ ] Pinned PiP layout (app resized vertically) works. [TT-DESIGN; TT-SCENES 2:59]

## H. Games (if applicable)

- [ ] Playable in every pose; fills the screen on pose change; aspect-ratio change preferred over letterbox/pillarbox; padding filled with artwork if unavoidable; text/control sizes consistent. [HIG-DUO "Make your game playable"]

## H2. App Store (if shipping)

- [ ] iPhone Duo screenshots prepared at 1398 × 2034 / 2034 × 1398 (outer) and 2007 × 2853 / 2853 × 2007 (inner) pixels; app previews at the accepted 886 × 1920 / 1920 × 886. [ASC-SHOTS; ASC-PREVIEWS]
- [ ] Team knows: submitting Duo screenshots earns the "optimized for iPhone Duo" badge; from April 2027 Duo screenshots are required (news: "any apps or games submitted"; spec page: "any app using the iOS 27.1 SDK or later"). [NEWS-1005; ASC-SHOTS]
- [ ] Assets checked in App Store Connect's preview tool; featuring nomination considered (Helpful Details → optimized for iPhone Duo, all poses). [DUO-PREPARE]

## I. Things that must be surfaced, not assumed

- [ ] Nowhere does generated code or documentation state point dimensions, scale factor, bar width, hinge thresholds, or safe-area values for iPhone Duo. See `device-facts.md` "NOT documented". 
- [ ] Symbols with no DocC page (`onHingeChange` / `DeviceHinge`, `UIBarButtonItem.badge`, `UICornerConfiguration`, outer/inner camera device types) are described as "declared in the iOS 27.1 SDK, not yet documented", not as fully documented. See `api-index.md` 🧩 rows.
- [ ] Any pixel figure quoted for the inner display names its source, because Apple's tech specs (1878 × 2670) and App Store Connect (2007 × 2853) disagree. [`device-facts.md`]
