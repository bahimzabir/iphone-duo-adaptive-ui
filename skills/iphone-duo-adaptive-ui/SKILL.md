---
name: iphone-duo-adaptive-ui
description: Build, adapt, or review SwiftUI and UIKit interfaces for iPhone Duo, Apple's folding iPhone with an outer display and a hinged inner display. Use when a task mentions iPhone Duo, a foldable or folding iPhone, the hinge or fold, inner or outer display, device poses, vertical bars or side toolbars, ArrangementView, reserved regions, Split View on iPhone, getting an app ready for iPhone Duo, iPhone Duo App Store screenshots, or making an iOS app resizable across size classes. Grounded only in Apple's published material; says what Apple has not documented instead of guessing.
license: MIT (skill text). Quoted Apple documentation remains Apple's; see LICENSE in the repository root.
metadata:
  author: Abderrahim
  version: "0.3.1"
  sources-fetched: "2026-09-10, re-verified 2026-09-22 and 2026-10-26"
  status: "iOS 27.1 released; iPhone Duo on sale since 2026-10-23; Xcode 27.1 (27A9275) SDK used to verify symbols without DocC pages"
---

# iPhone Duo adaptive UI

## Ground rules (read first)

1. **Only Apple sources.** HIG "Designing for iPhone Duo"; the article "Preparing your app for iPhone Duo"; the "Get ready for iPhone Duo" hub and its "Three steps" page; the October 2026 news item and App Store Connect specs; the iOS 27.1 DocC reference and released SDK; the six Tech Talks; the three Group Labs; Apple's own `app-resizability` skill. `references/sources.md` lists every URL. Do not supplement with blog posts or memory. Spoken Group Lab answers rank below written pages; the `group-lab-day*.md` files mark hedged ones.
2. **Do not invent numbers or APIs.** Apple has not published point dimensions, scale factor, vertical-bar width, hinge angle thresholds, or safe-area values. Its two published inner-display pixel figures disagree (tech specs 1878 × 2670; App Store Connect 2007 × 2853). If a task needs one, say so and ask the user; never derive (e.g. pixels ÷ 3). → `references/device-facts.md` "NOT documented".
3. **Some symbols are SDK-only.** `onHingeChange` / `DeviceHinge`, `UIBarButtonItem.badge`, `UICornerConfiguration` and the outer/inner camera device types have no DocC page but are declared in the released iOS 27.1 SDK (🧩 rows in `references/api-index.md`). When you use one, say "declared in the SDK, not yet documented".
4. **Dated claims expire.** Compare today with `sources-fetched` above. If more than three months have passed, re-probe the 🧩 rows, re-fetch the HIG page and the "Three steps" page, and re-check App Store deadlines before relying on them. Phrase deadlines as what Apple stated and when (e.g. "Apple said in October 2026 that iPhone Duo screenshots become required in April 2027").
5. **Scope is SwiftUI and UIKit.** Apple provides no iPhone Duo guidance for web, React Native, Flutter, or Unity (games get one HIG paragraph). If the stack is not native, say so and ask before applying anything here by analogy.
6. **Quote, don't paraphrase, when precision matters.** The reference files keep Apple's wording; reuse it.

## Fast facts

| Topic | Apple's statement |
|-------|-------------------|
| Displays | Outer (closed) and inner (open), each with its own front camera; center hinge. Outer display is "wider and shorter" than other iPhones. |
| Size classes | Outer portrait: compact W / regular H. Outer landscape: compact W / compact H. Inner: regular W / regular H in any orientation. Split View and pinned PiP change these at runtime. |
| Where bars go | Outer display and inner landscape: toolbars, tab bars, nav controls move to a vertical bar on the side (trailing edge; each app's outer edge in Split View; same physical side in RTL). Inner portrait keeps horizontal bars. |
| Vertical bar, top to bottom | Dynamic Island, status bar, toolbar (nav controls, then prominent actions, then remaining groups), tab bar. |
| Reserved regions | Outer camera (always; grows with Live Activities), inner camera (only while the camera is active), folding region (only when partially folded; zero width when flat). |
| Adapts for free | Alerts, action sheets, menus, context menus, popovers, sheets, toolbar buttons, `NavigationSplitView` / `UISplitViewController`, `TabView` / `UITabBarController`, `List`, `ScrollView`. |
| Orientation | Never a layout input. The outer display honors supported orientations; the inner display does not and is always regular/regular. Compare your own bounds' width and height instead. |

## Workflow

Follow in order. Each step names its reference file.

### 1. Confirm scope and SDK, run Apple's skill first
- Native SwiftUI/UIKit? If not, stop (rule 5).
- Target the iOS 27.1 SDK. Older SDKs: iOS 26 or earlier runs centered with empty space when open; iOS 27 fills most of the inner display but avoids the status bar; only 27.1 goes edge to edge with vertical bars. No opt-out once linked on iOS 27.
- Test with Apple's three tools: the iOS resizable simulator in Device Hub; iPhone Mirroring on macOS 27 resized "to extreme sizes in both directions"; the iPhone Duo simulator (open / close / rotate / fold, drag to each side in Split View, Previews "Display" override).
- For "get my app ready for iPhone Duo", run Apple's `app-resizability` skill first (Xcode coding assistant, or `xcrun agent skills export --output-dir <abs path>`). It rewrites `UIScreen.main`, orientation, idiom, safe-area and lifecycle code and checks three Info.plist keys. Then continue here. → `references/apple-app-resizability-skill.md`, `references/device-facts.md`

### 2. Make layout decisions on size classes only
- Read `horizontalSizeClass` / `verticalSizeClass` (SwiftUI environment, UIKit trait collection).
- Never branch on idiom, orientation (including `effectiveGeometry.interfaceOrientation`), device model, or size constants like `bounds.height == 844`.
- For aspect questions, compare the width and height of `view.bounds`, `superview.bounds`, or the `GeometryReader` size.
- Re-read size in `layoutSubviews` / `viewDidLayoutSubviews` / `onGeometryChange`, never once at launch or in `viewIsAppearing`.
- Two layouts cover every pose: compact width (outer) and regular width (inner). Let the compact layout expand; do not redesign per pose.
- Scale from `traitCollection.displayScale` / `@Environment(\.displayScale)`. Screen only via `view.window?.windowScene?.screen`, never stored. Windows via `UIWindow(windowScene:)`.
- Full-bleed hero media: choose fill vs fit by size class or aspect ratio, or set a focal point.
- Corners: `ConcentricRectangle` (SwiftUI) / `UICornerConfiguration` (UIKit). → `references/design-principles.md` §1

### 3. Use standard containers first
- Hierarchy: `NavigationSplitView` / `UISplitViewController`. Collapses closed, expands open, evens its columns when folded.
- Tabs: `TabView` / `UITabBarController`. Optional sidebar on the inner display (`.defaultTabBarPlacement(.sidebar)` / `sidebar.preferredPlacement = .sidebar`) for information-dense apps.
- Presentations: system sheets, alerts, menus, popovers get fold avoidance automatically. Inner-display sheets: `.presentationPlacement(.leading/.center/.trailing)` / `preferredPlacement`. Trailing gets a vertical bar; centered and leading keep horizontal bars.
- Never build primary bars from `UIToolbar`, `UINavigationBar`, `UITabBar`. Their items stay horizontal. → `references/arrangement-views-and-reserved-regions.md` "Split views"

### 4. Respect asymmetric safe areas and margins
- Interactive and foreground content stays inside the safe area. Only background art uses `ignoresSafeArea()` / `view.bounds`.
- Handle each edge on its own: `view.bounds.inset(by: view.safeAreaInsets)`. Never `width - left * 2`. No inset-threshold tests.
- `safeAreaInsets.left` is physical. Use `safeAreaLayoutGuide.leadingAnchor` / `.trailingAnchor` when you mean a direction.
- Custom bar placement reads `traitCollection.verticalBarEdge`. Outside layout methods, register `systemTraitsAffectingVerticalBarEdge`.
- On the outer display, content aligned to horizontal safe-area insets is offset from the vertical bar automatically.
- Full-width centering is only for non-scrolling immersive UI whose interactive elements cannot land under the bar, Dynamic Island, or status bar. Mixed pattern: full-width background, inset scrollable foreground.
- Check with Reduce Transparency on (the system draws an opaque rectangle behind the vertical bar). → `references/design-principles.md` §3–4

### 5. Get the vertical bar right
Apply `.toolbar` inside `NavigationStack` / `NavigationSplitView`; UIKit items on `navigationItem` inside `UINavigationController`. Then audit every item:
- Every non-text item has a title and a symbol (`Label` / `UIBarButtonItem`). Prefer symbols.
- Inline counts become badges: `.badge()` / `item.badge = .count()`.
- Items that toggle between symbol and text: `.axisBehavior(.horizontalOnly)`.
- Custom views that can go vertical: `.axisBehavior(.verticalPreferred)`, sized to the fixed bar width. Read `toolbarVerticalEdge` / `verticalBarEdge` to adapt.
- Custom back/close: `.cancellationAction` (UIKit: leading item, `leftItemsSupplementBackButton = false`).
- Prominent action: `.topBarPinnedTrailing` / `pinnedTrailingGroup`.
- Group with `ToolbarItemGroup` / `UIBarButtonItemGroup`. No manual spacers.
- Fold custom overflow into `ToolbarOverflowMenu` / `additionalOverflowItems`. Ellipsis only for overflow.
- Set `visibilityPriority` (groups first) so frequent and badged items overflow last. Default order is bottom to top.
- Compression: default keeps the tab bar. Task-oriented views: `.toolbarVerticalCompressionBehavior(.prefersToolbarItems)` / `verticalBarCompressionBehavior = .prefersBarItems`.
- Opt out (`.toolbarVerticalBehavior(.disabled)` / `preferredVerticalBarBehavior`) only for Apple's cases: a fullscreen video player, a non-scrolling layout like Calculator, a sheet with a single control. "Treat it as a stable choice"; never toggle per view state. → `references/vertical-bars-and-toolbars.md`

### 6. Use the inner display's width
Pick one of Apple's three options and keep the hierarchy identical to the outer display:
1. A split view that exposes one more level of hierarchy.
2. A stacked layout that becomes two columns in regular width (Music).
3. The tab bar as a sidebar (information-dense apps only).

When a second column appears on unfold, do not auto-select content with side effects (such as marking a conversation read). Show a content-unavailable view until the user picks. Keep text within readable-content width. → `references/design-principles.md` §5, `references/group-lab-day3.md`

### 7. Handle the fold (partially open inner display)
- Audit centered layouts: convert to two columns or define a displacement rule.
- Move the minimum. Move related elements together. Scrolling content never displaces.
- Grids: even column count, extra spacing around the hinge, outer margins preserved.
- Flat vs folded: Apple's default shape is a 35/65 split that becomes 50/50 when folded. Avoid large transitions.
- Prefer scrolling perpendicular to the hinge; parallel scrolling is hard to reach at the crease. → `references/group-lab-day3.md`
- Two-view layouts: `ArrangementView` / `UIArrangementViewController` inside the navigation container. Never inside `NavigationSplitView`, `List`, `ScrollView` or any container that could hide part of it. Never wrapping navigation containers.
  - Side by side, nothing may be obscured: `.arrangementViewStyle(.split)`; restrict with `.split.axes(.horizontal)`. If the primary axis cannot split, only the primary view shows.
  - Layered, partial obscuring acceptable: `.arrangementViewStyle(.overlay)`; read `overlayArrangementZIndex` (`> 0` means on top).
- Custom manual layouts: `proxy.reservedRegions(kind:options:layoutDirectionBehavior:)` / `view.reservedRegions(kind:options:)`. Use `frame` (includes `margins`) and `isActive`. `.includeInactive` plans while flat. `kind: .occlusion` for the camera. Frames are RTL-mirrored by default; pass `.fixed` for hardware coordinates. → `references/arrangement-views-and-reserved-regions.md`

### 8. Hinge, scenes, multitasking
- Hinge drives effects only, never layout. SwiftUI: `onHingeChange(isEnabled:_:)` → `DeviceHingeContext.hinge: DeviceHinge?` (SDK-declared, no DocC page). UIKit: `UIHingeInteraction` + `UIHinge` (documented). Handle a nil hinge. Prefer `status` over `angle` (`Angle` in SwiftUI, radians in UIKit).
- Apple's layering: system components, then `ArrangementView`, then reserved regions, then raw angle. Pick the highest layer that fits.
- All apps get Split View and pinned-PiP resizing. Multi-scene apps handle scene-request failure (new windows only on the inner display) and use `UIWindowScene.ActivationAction`.
- Camera apps: `CameraCaptureAccessory` for outer-display companion UI (needs an active capture session); virtual front camera or a direction coordinator. → `references/hinge-multitasking-scenes.md`

### 9. Games
Fill the screen in every pose even when orientation-locked. Change aspect ratio rather than letterbox. Fill unavoidable padding with artwork. Keep text and control sizes consistent. Nothing more is documented.

### 10. Verify and ship
Run `references/audit-checklist.md`. Report each failing item with file and view. Name any 🧩 symbol you used. For App Store builds: iPhone Duo screenshots (1398 × 2034 outer, 2007 × 2853 inner, both orientations) earn the "optimized for iPhone Duo" badge and, per Apple's October 2026 statement, become required in April 2027. → `references/apple-get-ready-prepare.md`

## Anti-patterns

Each line contradicts an Apple statement. Grep for them.

- `UIScreen.main`, `UIWindow(frame: UIScreen.main.bounds)`, a stored `UIScreen`
- `bounds.height == 844` or any screen-size constant, fixed width, or breakpoint
- Size read once at launch or in `viewIsAppearing`
- `userInterfaceIdiom == .pad` / `.phone` deciding layout
- `UIDevice.current.orientation`, `statusBarOrientation`, `interfaceOrientation`, `effectiveGeometry.interfaceOrientation` deciding layout
- `horizontalSizeClass == .regular` treated as "iPad"
- `safeAreaInsets.left * 2` or any symmetric-inset assumption; inset-threshold tests
- `UITabBar` / `UIToolbar` / `UINavigationBar` as primary bars; manual toolbar spacers
- Toolbar item with a symbol but no title; text-only item where a symbol works; text item forced vertical
- Vertical bar disabled outside Apple's listed cases
- Navigation containers inside `ArrangementView`; `ArrangementView` inside `List` / `ScrollView`
- Hinge angle used for layout
- Scrolling feeds displaced on fold; large rearrangements on fold
- A per-pose layout that changes functionality or hierarchy
- `.scaleAspectFill` hero media unchecked on the wider display
- Point sizes, scale factor, bar width, hinge thresholds, or an unsourced inner-display pixel count stated as fact
- A promised opt-out from resizability on the iOS 27 SDK; Duo guidance for web, React Native, or Flutter

## Minimal code map

Each snippet carries one tag: **verbatim** (Apple's code as published), **composed** (assembled from Apple's samples, not itself a sample), **SDK-only** (compiles against the released SDK; no DocC page).

```swift
// [verbatim] Size classes — TT-PREPARE
@Environment(\.horizontalSizeClass) private var horizontalSizeClass
traitCollection.horizontalSizeClass

// [verbatim] Edge-independent safe area — TT-PREPARE
let content = view.bounds.inset(by: view.safeAreaInsets)

// [composed] Vertical-bar-ready toolbar — from TT-BARS samples
.toolbar {
    ToolbarItem { Button { … } label: { Label("Compose", systemImage: "square.and.pencil") } }
        .visibilityPriority(.high)
    ToolbarOverflowMenu { Button("Scan") { … } }
}

// [composed] Two-view fold-aware layout — from TT-POSE samples
NavigationStack {
    ArrangementView { PlayerView() } secondary: { UpNextView() }
        .arrangementViewStyle(.split.axes(.horizontal))
}

// [SDK-only] Hinge status — TT-SCENES shape, names from SwiftUICore.swiftinterface
.onHingeChange { _, context in
    if let hinge = context.hinge, hinge.status == .partiallyOpen { /* effect only, never layout */ }
}

// [verbatim] Custom displacement — DocC sample
GeometryReader { proxy in
    RegionAvoidingLayout(regions: proxy.reservedRegions(kind: .occlusion)) {
        ForEach(items) { item in ItemView(item) }
    }
}
```


## References

| File | Use it for |
|------|-----------|
| `references/hig-designing-for-iphone-duo.md` | Apple's HIG page, full text. Quote from here. |
| `references/apple-preparing-your-app-for-iphone-duo.md` | Apple's developer article, full text. Quote from here. |
| `references/apple-get-ready-prepare.md` | Apple's "Three steps" page (full text), October 2026 news, App Store screenshot and preview specs, hub inventory. |
| `references/apple-app-resizability-skill.md` | How to export and run Apple's Xcode 27.1 `app-resizability` skill; its prerequisites and key rules, quoted. |
| `references/device-facts.md` | Anatomy, size-class table, SDK behavior, orientation quotes, apple.com specs, NOT-documented list. |
| `references/design-principles.md` | Displacement, consistency, outer and inner display patterns, sheets, games. |
| `references/vertical-bars-and-toolbars.md` | Side bars, item axis, overflow, priority, opt-out, with code. |
| `references/arrangement-views-and-reserved-regions.md` | `ArrangementView`, split vs overlay, reserved-region queries, split views, with code. |
| `references/hinge-multitasking-scenes.md` | Hinge API, Split View, scenes, scene accessories, camera direction. |
| `references/group-lab-day1.md`, `-day2.md`, `-day3.md` | Apple panel Q&A, one file per Group Lab, question list at the top; hedges marked. Day 1: orientation, custom tab bars, accessibility. Day 2: state, sheets, compression, idiom. Day 3: fold avoidance, scroll direction, tab bar vs sidebar, lifecycle. |
| `references/api-index.md` | Every symbol with source, DocC status (✅ / 🧩 / ❌) and confirmed signature. |
| `references/audit-checklist.md` | Pass/fail review list. |
| `references/sources.md` | All URLs, fetch dates, blocked and missing pages. |
