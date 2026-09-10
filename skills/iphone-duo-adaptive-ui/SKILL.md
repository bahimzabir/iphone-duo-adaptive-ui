---
name: iphone-duo-adaptive-ui
description: Build, adapt, or review SwiftUI and UIKit interfaces for iPhone Duo, Apple's folding iPhone with an outer display and a hinged inner display. Use when a task mentions iPhone Duo, foldable or folding iPhone, the hinge or fold, inner/outer display, device poses, vertical bars or side toolbars, ArrangementView, reserved regions, Split View on iPhone, or making an iOS app resizable across size classes. Grounded only in Apple's Human Interface Guidelines and September 2026 Tech Talks; flags what Apple has not documented instead of guessing.
license: MIT (skill text). Quoted Apple documentation remains Apple's; see LICENSE in the repository root.
metadata:
  author: Abderrahim
  version: "0.1.0"
  sources-fetched: "2026-09-10"
  status: "pre-release — iOS 27.1 SDK and Apple API reference not yet published at authoring time"
---

# iPhone Duo adaptive UI

## Ground rules (read first)

1. **Only Apple sources.** Everything here comes from the HIG page "Designing for iPhone Duo" and Apple's six iPhone Duo Tech Talks, fetched 2026-09-10. `references/sources.md` lists every URL. Do not supplement with blog posts or memory.
2. **Do not invent numbers or APIs.** Apple has not published point dimensions, scale factor, vertical-bar width, hinge angle thresholds, or safe-area values. If a task needs one, say so and ask the user; never derive them (e.g. pixels ÷ 3). See `references/device-facts.md` → "NOT documented".
3. **iOS 27.1 APIs are pre-release.** `ArrangementView`, reserved regions, vertical-bar modifiers, hinge APIs and scene accessories exist only in Tech Talk code samples; their DocC pages returned 404 on 2026-09-10 and Xcode 27.1 beta was "coming later this month". Use the spellings in `references/api-index.md`, tell the user they need the iOS 27.1 SDK, and recommend verifying against the SDK.
4. **Scope is SwiftUI and UIKit.** Apple provides no iPhone Duo guidance for web, React Native, Flutter, or Unity (games get one HIG paragraph). If the stack is not native, state that and ask before applying anything here by analogy.
5. **Quote, don't paraphrase, when precision matters.** The reference files keep Apple's wording; reuse it.

## Fast facts

| Topic | Apple's statement |
|-------|-------------------|
| Displays | Outer (device closed) and inner (device open), each with its own front camera; center hinge. Outer display is "wider and shorter" than other iPhones. |
| Size classes | Outer portrait: compact W / regular H. Outer landscape: compact W / compact H. Inner: regular W / regular H (any orientation). Split View and pinned PiP change these at runtime. |
| Where bars go | Outer display and inner-landscape: toolbars, tab bars, nav controls move to a **vertical bar on the side** (Apple's outer-display diagram labels it the trailing edge and TT-DESIGN says "the right side"; each app's outer edge in Split View; hardware-aligned, so the same physical side in RTL). Inner portrait keeps horizontal bars. |
| Vertical bar contents (top→bottom) | Dynamic Island, status bar, toolbar (nav controls first, then prominent actions, then remaining groups), tab bar. |
| Reserved regions | Outer camera (always; grows with Live Activities), inner camera (only when camera active), folding region (only when partially folded; zero width when flat). |
| What adapts for free | Alerts, action sheets, menus, context menus, popovers, sheets, toolbar buttons, `NavigationSplitView`/`UISplitViewController`, `TabView`/`UITabBarController`, `List`, `ScrollView`. |
| Orientation | Do not use it for layout. See the quoted ambiguity in `references/device-facts.md`. |
| SDK gates | Pre-27 SDK: runs, avoids status bar area. 27 SDK: extends left of status bar on inner display. 27.1 SDK: edge-to-edge plus vertical bars. |

## Workflow

Follow in order. Each step names its reference file.

### 1. Confirm scope and SDK
- Native SwiftUI/UIKit? If not, stop and tell the user Apple has no guidance for their stack (rule 4).
- Target iOS 27.1 SDK. Tell the user to test in Xcode 27.1's iPhone Duo simulator in Device Hub, using its open / close / rotate / fold controls, and to drag the app to each side in Split View. → `references/device-facts.md`

### 2. Make layout decisions on size classes only
- Read `horizontalSizeClass` / `verticalSizeClass` (SwiftUI environment, UIKit trait collection). Never idiom, orientation, device model, or `UIScreen.main`.
- Two layouts cover every pose: compact width (outer) and regular width (inner). Let the compact layout expand; do not redesign per pose.
- Replace `UIScreen.main.scale` with `traitCollection.displayScale`; get a screen only via `windowScene.screen`.
- Corners: `ConcentricRectangle` (SwiftUI) / `UICornerConfiguration` (UIKit). → `references/design-principles.md` §1, `references/hinge-multitasking-scenes.md` "Screens"

### 3. Use standard containers first
- Hierarchy: `NavigationSplitView` / `UISplitViewController` — collapses to one pane closed, expands open, evens its columns when folded.
- Tabs: `TabView` / `UITabBarController`; optional sidebar on the inner display (`.defaultTabBarPlacement(.sidebar)` / `sidebar.preferredPlacement = .sidebar`) for information-dense apps.
- Presentations: system sheets, alerts, menus, popovers — they get fold avoidance automatically.
- Do **not** build primary bars from `UIToolbar`, `UINavigationBar`, `UITabBar`; their items are ignored for vertical layout. → `references/arrangement-views-and-reserved-regions.md` "Split views"

### 4. Respect asymmetric safe areas and margins
- Interactive/foreground content inside the safe area; only background art uses `ignoresSafeArea()` / `view.bounds`.
- Handle each edge independently: `view.bounds.inset(by: view.safeAreaInsets)`, never `width - left * 2`.
- Outer display: content is offset from the vertical bar automatically when aligned to horizontal safe-area insets. Full-width centering is only for non-scrolling immersive UI whose interactive elements cannot land under the bar, Dynamic Island, or status bar. Mixed pattern: full-width background, inset scrollable foreground. → `references/design-principles.md` §3–4

### 5. Get the vertical bar right
Apply `.toolbar` inside `NavigationStack`/`NavigationSplitView`; UIKit items on `navigationItem` inside `UINavigationController`. Then audit every item:
- Title **and** symbol on every non-text item (`Label` / `UIBarButtonItem`). Prefer symbols; convert inline counts to `.badge()` / `item.badge = .count()`.
- Symbol↔text togglers: `.axisBehavior(.horizontalOnly)`. Custom views that can go vertical: `.axisBehavior(.verticalPreferred)` and fit the fixed bar width; read `toolbarVerticalEdge` / `verticalBarEdge` to adapt.
- Order: custom back/close via `.cancellationAction` (UIKit leading item, `leftItemsSupplementBackButton = false`); prominent action via `.topBarPinnedTrailing` / `pinnedTrailingGroup`; keep other groups as they were; group with `ToolbarItemGroup` / `UIBarButtonItemGroup`, no manual spacing.
- Overflow: merge custom overflow into `ToolbarOverflowMenu` / `additionalOverflowItems`; ellipsis only for overflow; set `visibilityPriority` (groups first) so frequent and badged items overflow last (default order is bottom-to-top).
- Compression: default keeps the tab bar (navigation-focused). Task-oriented views: `.toolbarVerticalCompressionBehavior(.prefersToolbarItems)` / `verticalBarCompressionBehavior = .prefersBarItems`.
- Opt out (`.toolbarVerticalBehavior(.disabled)` / `preferredVerticalBarBehavior`) only for Apple's two cases: single-page bottom-heavy layouts like Calculator, or a sheet with a single control. → `references/vertical-bars-and-toolbars.md`

### 6. Use the inner display's width
Pick one of Apple's three options; keep the hierarchy identical to the outer display:
1. Split view exposing one more level of hierarchy.
2. Vertically stacked layout that becomes two columns in regular width (Music example).
3. Tab bar as sidebar (information-dense apps only). → `references/design-principles.md` §5

### 7. Handle the fold (partially open inner display)
- Audit centered layouts: convert to two columns or define a displacement rule. Move the minimum; move related elements together; scrolling content never displaces. Grids: even column count, extra spacing around the hinge, outer margins preserved.
- Two-view layouts: `ArrangementView` (SwiftUI) / `UIArrangementViewController` (UIKit) inside the navigation container, never inside `List`/`ScrollView`, never wrapping navigation containers.
  - `HStack`/`VStack`-like, main–detail, nothing may be obscured → `.arrangementViewStyle(.split)`; restrict with `.split.axes(.horizontal)`; if the primary axis can't split, only the primary view shows.
  - `ZStack`-like, foreground/background, partial obscuring acceptable → `.arrangementViewStyle(.overlay)`; read `overlayArrangementZIndex` (`> 0` = on top) to switch collapsed/expanded.
- Custom manual layouts: query `proxy.reservedRegions(kind: .division)` / `view.reservedRegions(kind: .division)` and use each region's `frame`; `.includeInactive` to plan (e.g. even columns) while flat; `kind: .occlusion` for the camera. → `references/arrangement-views-and-reserved-regions.md`

### 8. Hinge, scenes, multitasking
- Hinge angle/status (`onHingeChange` / `UIHingeInteraction`) drives effects only, never layout; handle a nil hinge.
- All apps get Split View and pinned-PiP resizing; multi-scene apps must handle scene-request failure (new windows only on the inner display) and use `UIWindowSceneActivationAction`.
- Camera apps: consider `CameraCaptureAccessory` for outer-display companion UI; use the virtual front camera or a direction coordinator. → `references/hinge-multitasking-scenes.md`

### 9. Games
Fill the screen in every pose even when orientation-locked; change aspect ratio rather than letterbox; fill unavoidable padding with artwork; keep text and control sizes consistent. Nothing more is documented.

### 10. Verify
Run `references/audit-checklist.md` against the result. Report each failing item with file and view. State explicitly which iOS 27.1 symbols are unverified against a shipping SDK.

## Anti-patterns (each contradicts an Apple statement)

- `UIScreen.main` anywhere; screen-size constants; fixed widths or breakpoints.
- Layout branches on `userInterfaceIdiom`, `interfaceOrientation`, or device model.
- `safeAreaInsets.left * 2` or any symmetric-inset assumption.
- Custom `UIToolbar` / `UINavigationBar` / `UITabBar` as primary bars; manual toolbar spacers.
- Toolbar items with a symbol but no title; text-only items where a symbol works; cart-style items with meaningful text forced vertical.
- Overriding vertical bar placement outside Apple's two exceptions.
- A per-pose custom layout that changes functionality or hierarchy.
- Navigation containers inside `ArrangementView`; `ArrangementView` inside `List`/`ScrollView`.
- Using hinge angle to lay out views.
- Displacing scrolling feeds/articles when folded; large rearrangements on fold.
- Stating point sizes, scale factor, bar widths, or hinge thresholds for iPhone Duo.

## Minimal code map

```swift
// Size classes (SwiftUI / UIKit)
@Environment(\.horizontalSizeClass) private var horizontalSizeClass
traitCollection.horizontalSizeClass

// Edge-independent safe area (UIKit)
let content = view.bounds.inset(by: view.safeAreaInsets)

// Vertical-bar-ready toolbar item (SwiftUI) — composed from Apple's TT-BARS samples, not a verbatim sample
.toolbar {
    ToolbarItem { Button { … } label: { Label("Compose", systemImage: "square.and.pencil") } }
        .visibilityPriority(.high)
    ToolbarOverflowMenu { Button("Scan") { … } }
}

// Two-view fold-aware layout (SwiftUI, iOS 27.1)
NavigationStack {
    ArrangementView { PlayerView() } secondary: { UpNextView() }
        .arrangementViewStyle(.split.axes(.horizontal))
}

// Custom displacement (SwiftUI, iOS 27.1)
GeometryReader { proxy in
    let folds = proxy.reservedRegions(kind: .division).map(\.frame)
    …
}
```

Full, verbatim Apple samples with timestamps are in the reference files.

## References

| File | Use it for |
|------|-----------|
| `references/hig-designing-for-iphone-duo.md` | Apple's HIG page, full text. Quote from here. |
| `references/device-facts.md` | Anatomy, size-class table, SDK gating, orientation quotes, apple.com specs, **NOT documented** list. |
| `references/design-principles.md` | Displacement, consistency, outer/inner display patterns, sheets, games. |
| `references/vertical-bars-and-toolbars.md` | Everything about side bars, item axis, overflow, priority, opt-out, with code. |
| `references/arrangement-views-and-reserved-regions.md` | `ArrangementView`, split vs overlay, reserved-region queries, split views, with code. |
| `references/hinge-multitasking-scenes.md` | Hinge API, Split View, scenes, scene accessories, camera direction. |
| `references/api-index.md` | Every symbol with source, timestamp, and DocC status. |
| `references/audit-checklist.md` | Pass/fail review list. |
| `references/sources.md` | All URLs, fetch date, 404 list. |
