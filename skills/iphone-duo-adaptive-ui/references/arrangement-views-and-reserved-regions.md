# Arrangement views and reserved regions (iOS 27.1)

Sources: HIG-DUO "Dynamic layouts" (Reserved regions, Split views, Arrangement views); TT-POSE transcript and on-page sample code; TT-PREPARE. Code is Apple's, verbatim. **None of these APIs had a published DocC page on 2026-09-10**; treat names as pre-release and verify against the iOS 27.1 SDK before shipping. Provenance per symbol is in `api-index.md`.

## Reserved regions — the concept (HIG-DUO)

"In addition to standard considerations for safe areas, available space on iPhone Duo is shaped by *reserved regions*. These represent areas within the display that content avoids covering, or that components adapt to accommodate. These are familiar if your layout adapts to similar areas on other platforms, such as the window controls on iPad."

| Region | Presence | Behavior |
|--------|----------|----------|
| Outer front-facing camera | "always present" | "expands into the Dynamic Island for Live Activities. When controls are on the side, the system automatically accounts for it." |
| Inner front-facing camera | "only present when the camera is active" | "when the camera activates, the UI moves aside to indicate the presence of the camera." |
| Folding region | "conditional based on how a person uses the device" | "When the device is partially open, the folding region divides the inner display into multiple usable regions, excluding the region at the center as the display folds." |

"Many system components automatically adapt to reserved regions. Components like alerts, context menus, and sheets automatically move to account for the fold, while larger components like Split views adapt their columns' width and margins to match the symmetry of the inner display. For custom components, the reserved region APIs provide a way to reposition content away from reserved regions."

TT-PREPARE positions the API as the tool for custom bars and edge-to-edge UI: "a new API that lets your custom UI use as much of the available screen space as possible without colliding with system-provided UI... to safely position UI elements outside the safe area while maximizing usable space."

## Reserved regions — the API (TT-POSE 6:39–8:39)

Two kinds:
- **Division** regions — "divides a larger area into multiple smaller areas." The fold is a division region.
- **Occlusion** regions — "These don't divide areas; instead, they occlude them. Think of them as smaller frames in your view's bounds." The FaceTime camera is an occlusion region.

Active vs inactive:
- "By default, only active ones will be returned, but you can query for inactive ones using the includeInactive query option."
- "the fold's division region is only active when someone has folded the device. When flat, it's inactive and has a width of zero."
- The camera occlusion region "is active when the camera is active and inactive when the camera is inactive."
- Use of inactive regions: "in grid-like layouts, you could prefer even numbers of columns when a division region is present regardless of its active state."

Where to query: SwiftUI — a `GeometryProxy` from `GeometryReader` or the `onGeometryChange` modifier. UIKit — a method on `UIView`. Read each region's `frame` to use it in your own layout.

```swift
// Query reserved regions in SwiftUI  (TT-POSE 6:46)
GeometryReader { proxy in
  let regions = proxy.reservedRegions(
    kind: .division)
}
```

```swift
// Query reserved regions in UIKit  (TT-POSE 7:03)
let regions = view.reservedRegions(
  kind: .division)

// Query the frame to incorporate it into your own layout
let frames = regions.map(\.frame)
```

```swift
// Include inactive regions  (TT-POSE 7:22)
GeometryReader { proxy in
  let regions = proxy.reservedRegions(
    kind: .division, options: .includeInactive)

  let frames = regions.map(\.frame)
  // ...
}
```

```swift
// Query occlusion regions  (TT-POSE 8:07)
GeometryReader { proxy in
  let regions = proxy.reservedRegions(
    kind: .occlusion)

  let frames = regions.map(\.frame)
  // ...
}
```

Naming note: the code samples spell the call `reservedRegions(kind:)` / `reservedRegions(kind:options:)`; the spoken transcript says "reservedRegion method." TT-PREPARE names the types "ReservedRegion in SwiftUI" and "UIViewReservedRegion in UIKit." Prefer the code-sample spelling; confirm in the SDK.

When to adopt: "identify the highest priority manually laid-out controls in your views and consider adopting the ReservedRegions API to implement your own displacement where needed." (TT-POSE) Standard containers and presentations already handle the fold.

## Split views on iPhone Duo (HIG-DUO "Split views"; TT-PREPARE 5:01)

- "a split view expands on the inner display and collapses to a single pane on the outer display, the same way it adapts between regular and compact environments on other iPhone devices."
- "When built with standard components, split views adapt to reserved regions automatically, adjusting width and margins to adapt to the fold." Notes: sidebar narrower than detail when open; equal widths when partially folded. Reminders: "an even 50/50 split." (TT-POSE)
- `NavigationSplitView` / `UISplitViewController`: "When iPhone Duo is closed, columns will collapse to single-stack navigation. When open, columns will appear both tiled and as overlays." (TT-PREPARE)
- Tab bar → sidebar on the inner display (optional, "works best for information-dense apps"):

```swift
// Show a sidebar on the inner display  (TT-PREPARE 5:44)
// SwiftUI
TabView { … }
    .defaultTabBarPlacement(.sidebar)

// UIKit
tabBarController.sidebar.preferredPlacement = .sidebar
```

## Arrangement views — the concept (HIG-DUO "Arrangement views"; TT-POSE 9:20)

"An *arrangement view* is a layout container that holds two views inside it — a primary view and a secondary view — and dynamically organizes them based on display size, orientation, and reserved regions." It sits "in between... navigation and content containers." Apple describes an arrangement as a function from inputs — "the horizontal and vertical size class of the view, the aspect ratio of the view's width over its height, and whether there are any active division regions" — to outputs — "whether I should show the view at all, and if I do show the view, what's its frame?"

Two types (HIG-DUO):
- **Split** — "divides its area between its primary and secondary views. It splits horizontally when the arrangement is wider than it is tall, and splits vertically when the arrangement is taller than it is wide."
- **Overlay** — "positions the primary and secondary views on top of one another. When the display is partially folded, the views move to occupy each side; otherwise the primary view moves atop the secondary view."

"You can limit which axes a split arrangement uses, and collapse the secondary view in an overlay arrangement when you don't want it to appear."

Podcasts example (TT-POSE): when the transcript is hidden on a folded Duo, "the Now Playing view is not centered like it was on iPad. Instead, it stays constrained to the left region defined by the fold."

## Arrangement views — the API (TT-POSE 11:17–14:21)

```swift
// Add an ArrangementView  (TT-POSE 11:23)
// SwiftUI
var body: some View {
  NavigationStack {
    ArrangementView {
      PlayerView()
    } secondary: {
      UpNextView()
    }
  }
}
```

```swift
// Add a UIArrangementViewController  (TT-POSE 11:26)
// UIKit
let arrangementVC = UIArrangementViewController()
let navController = UINavigationController(rootViewController: arrangementVC)

let playerVC = PlayerViewController()
arrangementVC.setViewController(playerVC, for: .primary)

let upNextVC = UpNextViewController()
arrangementVC.setViewController(upNextVC, for: .secondary)
```

```swift
// Specify the split arrangement style (the default)  (TT-POSE 12:00)
// SwiftUI
var body: some View {
  NavigationStack {
    ArrangementView {
      PlayerView()
    } secondary: {
      UpNextView()
    }
    .arrangementViewStyle(.split)
  }
}
```

```swift
// Restrict the split to one axis  (TT-POSE 12:41)
// SwiftUI
var body: some View {
  NavigationStack {
    ArrangementView {
      PlayerView()
    } secondary: {
      UpNextView()
    }
    .arrangementViewStyle(
      .split.axes(.horizontal))
  }
}
```

Axis rule: "If the split arrangement cannot split among an axis, and it's the primary axis, the arrangement view chooses to only show a single view." Example: view taller than wide (primary axis vertical) but restricted to horizontal → only the primary view (PlayerView) is shown.

```swift
// Update the arrangement in UIKit  (TT-POSE 13:07)
// UIKit
let arrangementVC = UIArrangementViewController()

// ...

arrangementVC.updateArrangement(.split.axes(.horizontal))
```

```swift
// Switch to the overlay arrangement  (TT-POSE 13:26)
// SwiftUI
var body: some View {
  NavigationStack {
    ArrangementView {
      UpNextView()
    } secondary: {
      PlayerView()
    }
    .arrangementViewStyle(.overlay)
  }
}
```

Overlay behavior: "prefers to position content above or below each other... if I fold the device, the overlay arrangement prefers to position the primary and secondary views side by side."

```swift
// Respond to the overlay Z index  (TT-POSE 14:07)
// SwiftUI
enum UpNextMinimization {
  case collapsed; case expanded
}

struct UpNextView: View {
  @Environment(\.overlayArrangementZIndex)
  private var zIndex: Int

  var body: some View {
    UpNextList(minimization: minimization)
  }

  var minimization: UpNextMinimization {
    zIndex > 0 ? .collapsed : .expanded
  }
}
```

```swift
// Read the Z index in UIKit  (TT-POSE 14:21)
// UIKit
let arrangementVC = UIArrangementViewController()

// ...

let primaryState = arrangementVC.state(for: .primary)
myModel.minimization = (primaryState?.zIndex ?? 0) > 0
  ? .collapsed : .expanded
```

## Choosing an arrangement (HIG-DUO; TT-POSE 14:39)

1. Follow existing patterns: `HStack`/`VStack` side-by-side or stacked layouts → split; `ZStack` layered layouts → overlay.
2. No existing pattern: overlay "when there's a clear foreground/background relationship" and partial obscuring is acceptable (Accessibility Reader: controls over scrollable text); split "when there's more of a main-detail relationship" and "neither of them are ever obscured" (Podcasts transcript).

## When NOT to use an arrangement view (HIG-DUO; TT-POSE 16:09)

- "Keep navigation outside of arrangement views. An arrangement view lays out content but doesn't handle navigation, so place navigation containers like navigation split views and tab views around it rather than within it."
- "avoid putting ArrangementViews inside of these scrollable containers" (List, ScrollView).
- If you need expand/collapse column navigation, that is `NavigationSplitView` / `UISplitViewController`, not an arrangement view (Maria's question at TT-POSE 9:20 frames the arrangement view as the tool for "a layout that's kind of like a split view, but I don't really need all the expanding and collapsing behavior").

## Audit sequence Apple recommends (TT-POSE 16:34)

1. "start by auditing your app's centered layouts. Consider whether you can make it a two-column layout, or what displacement pattern makes sense."
2. "If you're using standard system containers and presentations, you'll find you get a lot of behavior for free."
3. "if you're using a more custom horizontal split or an overlay layout, consider using ArrangementView."
4. "identify the highest priority manually laid-out controls in your views and consider adopting the ReservedRegions API to implement your own displacement where needed."
