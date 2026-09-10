# iPhone Duo readiness audit checklist

Use this to review an existing SwiftUI or UIKit app, or to self-check generated UI. Every line derives from an Apple statement (source tag in brackets; IDs in `sources.md`). Mark each item **pass / fail / n/a** and record the file or view concerned.

## A. Build and test setup

- [ ] App is built with the iOS 27.1 SDK (older SDKs do not get edge-to-edge layout or vertical bars). [TT-PREPARE 0:30]
- [ ] App has been run in the iPhone Duo simulator via Device Hub in every pose the controls offer: open, closed, rotated, folded. [TT-PREPARE 1:17]
- [ ] App has been tested in Split View multitasking on both the left and right side (vertical bar may be on either edge). [TT-PREPARE, "Respect safe areas" chapter, Device Hub drag test]
- [ ] Xcode 27.1's App Resizability skill has been run, if available. [TT-PREPARE 9:12]

## B. Layout decisions

- [ ] No layout decision keys off `userInterfaceIdiom`, interface orientation, or device model. Size classes only. [HIG-LAYOUT; TT-PREPARE 2:46]
- [ ] No fixed widths, breakpoints, or metrics tied to a specific screen. [HIG-DUO; TT-DESIGN 3:42]
- [ ] No `UIScreen.main` (or any static screen reference). Uses environment / trait collection / scene bounds, or `windowScene.screen`. [TT-PREPARE 3:57]
- [ ] `traitCollection.displayScale` used instead of `UIScreen.main.scale`. [TT-PREPARE 9:25]
- [ ] Both size-class layouts exist: compact-width (outer display) and regular-width (inner display). The regular-width layout is not a stretched compact one. [HIG-DUO; TT-DESIGN 7:34]
- [ ] Functionality and element state are identical across displays and poses; the inner display may add a hierarchy level but never removes one. [HIG-DUO "Create a consistent experience"]
- [ ] Screen-corner-hugging UI uses `ConcentricRectangle` / `UICornerConfiguration`. [TT-PREPARE 4:30]

## C. Safe areas and margins

- [ ] Interactive/foreground content is inside the safe area (SwiftUI default; UIKit `safeAreaLayoutGuide` or `safeAreaInsets`). [TT-PREPARE 6:06]
- [ ] Only background/full-bleed art uses `ignoresSafeArea()` / `view.bounds`. [TT-PREPARE 7:07]
- [ ] No code assumes left/right (or top/bottom) insets are equal; each edge handled independently. [TT-PREPARE 7:30]
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
- [ ] Arrangement views contain no navigation containers and are not inside `List` / `ScrollView`. [HIG-DUO; TT-POSE 16:09]
- [ ] Highest-priority manually laid-out controls query `reservedRegions(kind: .division)` and displace themselves. [TT-POSE 16:34]
- [ ] Camera-centric UI queries `reservedRegions(kind: .occlusion)` and keeps content clear when active. [TT-POSE 0:27, 8:07]
- [ ] Displacement is minimal: only what must move, moved together when related; scrolling content does not displace. [HIG-DUO; TT-POSE 2:26]

## G. Hinge, scenes, multitasking

- [ ] Hinge angle (`onHingeChange` / `UIHingeInteraction`) is used only for effects/interactions, never for layout; nil hinge handled. [TT-SCENES 1:18, 2:35]
- [ ] Multi-scene apps handle scene-request errors and use `UIWindowSceneActivationAction` (new windows only on the inner display). [TT-SCENES 3:38]
- [ ] Pinned PiP layout (app resized vertically) works. [TT-DESIGN; TT-SCENES 2:59]

## H. Games (if applicable)

- [ ] Playable in every pose; fills the screen on pose change; aspect-ratio change preferred over letterbox/pillarbox; padding filled with artwork if unavoidable; text/control sizes consistent. [HIG-DUO "Make your game playable"]

## I. Things that must be surfaced, not assumed

- [ ] Nowhere does generated code or documentation state point dimensions, scale factor, bar width, hinge thresholds, or safe-area values for iPhone Duo. See `device-facts.md` "NOT documented". 
- [ ] All iOS 27.1 symbols are flagged as pre-release pending DocC publication. See `api-index.md`.
