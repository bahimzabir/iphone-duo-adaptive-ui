# Vertical bars: toolbars, tab bars, and navigation controls on iPhone Duo

Sources: HIG-DUO "Vertical controls"; TT-BARS (transcript + on-page sample code); TT-DESIGN; TT-PREPARE; PREP (Apple's article); DocC pages verified 2026-09-22 (iOS 27.1 beta). Code samples are Apple's, reproduced verbatim. API provenance is in `api-index.md`.

## What the system does

- On the outer display, and on the inner display in landscape, "toolbars, tab bars, and navigation controls that are typically at the top and bottom of the display move to the side, preserving vertical space for content and keeping controls within easy reach. The exception is the inner display in portrait, which has enough vertical space to keep standard horizontal bars." (HIG-DUO)
- The side strip is shared: "Controls on the side include both system and app elements: the Dynamic Island, the status bar, the toolbar (including navigation buttons), and the tab bar." Apple's diagram lists them top to bottom in that order. (HIG-DUO)
- The bar is hardware-aligned: "they hold the same position relative to the camera on the outer display, and stay on the same side in right-to-left languages." (HIG-DUO) "The content adapts around it while the bar itself remains fixed." (TT-BARS)
- On the outer display, "Buttons and controls that normally sit at the top and bottom move to the right side of the display... to make them easier to reach with your right thumb." (TT-DESIGN)
- Split View multitasking: "each one places controls along its outer edge, so the left app has controls on the left." (HIG-DUO)
- Mapping from existing bars (TT-DESIGN): "Toolbar buttons at the top of your app move to the top of this vertical space, while toolbar buttons on the bottom move to the bottom on iPhone Duo. And if you have a tab bar, it stays bottom aligned." Between the top and bottom groups "the system provides a vertical space between items from the top and bottom bars to keep them distinct." (HIG-DUO)
- The vertical bar has "a fixed width" and "flexible item height" — the opposite of horizontal bars. (TT-BARS)
- "Navigation bars, toolbars, and tab bars lay out outside the safe area... Horizontal bars provide top and bottom insets, while vertical bars provide leading and trailing insets." (TT-PREPARE)

## Opt in (TT-BARS 2:00)

1. "Rebuild your app against the latest SDKs" (iOS 27.1 SDK — see `device-facts.md`).
2. Use bars owned by navigation containers.
   - SwiftUI: "pair the toolbar modifier with containers like NavigationStack or NavigationSplitView."
   - UIKit: "prefer using UINavigationController and UITabBarController, which manage their own bars... rather than creating a custom UIToolbar. When used to build custom bars, content from sub-components like UIToolbar, UINavigationBar, or UITabBar won't be considered."

```swift
// SwiftUI  (TT-BARS 2:24)
var body: some View {
    NavigationStack {
        ContentView()
            .toolbar {
                ToolbarItem(placement: .bottomBar) {
                    ...
                }
            }
    }
}
```

```swift
// UIKit — content from a custom UIToolbar won't be considered.  (TT-BARS 2:39)
// Prefer UINavigationController and UITabBarController,
// which manage their own bars.
let toolbar = UIToolbar()
toolbar.items = [...]
```

## Which containers participate — Apple's article version (PREP)

"The system handles the selection of horizontal and vertical bar presentation differently in some contexts:
- Inspectors. The system presents bars in inspectors horizontally.
- Split views. In a split view displaying multiple views, the system shows bars horizontally for the sidebar or content view, and vertically for the detail view.
- Sheets. On the outer display, the system presents bars vertically for sheets by default. Use `toolbarVerticalBehavior(_:)` in SwiftUI or `preferredVerticalBarBehavior` in UIKit to disable vertical presentation for your bars. For sheets on the inner display, the system presents the toolbar horizontally for centered or leading placements, and vertically for trailing placements. Set `presentationPlacement(_:)` in SwiftUI or `preferredPlacement` in UIKit to indicate where you want the system to place the sheet."

Also from PREP: "If your view has a hero or background image, extend it under a vertical bar using `backgroundExtensionEffect()` in SwiftUI, or `UIBackgroundExtensionView` in UIKit."

## Which containers participate (TT-BARS 3:09)

- "toolbar items can only move vertically when their container is positioned along the display edge."
- "In split views, only the detail column participates. Items from other columns remain horizontal."
- "inspectors don't receive their own [vertical bar] when expanded to avoid confusion."
- Sheets: outer display — "if your sheets already have a toolbar, it's displayed vertically." Inner display — "they're centered by default, and items remain horizontal. When you use the preferredPlacement API to change their position, sheets placed on the left remain without a vertical bar, while sheets placed on the right receive one."
- "accessory bars should remain attached to keyboard rather than moving to the vertical axis."
- Guidance: "keep them associated with their container." And per HIG-DUO: "When controls belong to a content area other than the one along the trailing edge, keep them with that area rather than moving them to the side." Mail keeps list controls above the list pane.

## Ordering in the vertical bar (HIG-DUO; TT-BARS 4:29)

Top to bottom:
1. Primary navigation — Back or Close. Navigation controller adds Back automatically. Custom back/close: SwiftUI `.cancellationAction` placement; UIKit a leading item with `leftItemsSupplementBackButton = false` (the default).
2. Prominent actions — e.g. Done. SwiftUI `.topBarPinnedTrailing`; UIKit `navigationItem.pinnedTrailingGroup`.
3. Remaining items "in their original groupings," with a system spacer separating former top-bar and bottom-bar placements.
4. Tab bar (bottom-aligned).

```swift
// Place a back or close button  (TT-BARS 5:00)
// SwiftUI
.toolbar {
    ToolbarItem(placement: .cancellationAction) {
        ...
    }
}

// UIKit
navigationItem.leftItemsSupplementBackButton = false
navigationItem.leadingItemGroups
    = [UIBarButtonItemGroup(...)]
```

```swift
// Pin prominent actions to the trailing edge  (TT-BARS 5:24)
// SwiftUI
.toolbar {
    ToolbarItem(placement: .topBarPinnedTrailing) {
        ...
    }
}

// UIKit
navigationItem.pinnedTrailingGroup
    = UIBarButtonItemGroup(...)
```

"Keep controls consistent across device poses... keep controls' relative positions as similar as possible so people don't have to relearn where actions live." (HIG-DUO)

## Preparing item content (HIG-DUO; TT-BARS 5:56–10:07)

- Always give every non-text item **both a title and a symbol**: "Include a title even when an item shows a symbol, because the system uses the title in overflow menus and expanded forms." SwiftUI `Label`; UIKit `UIBarButtonItem` title + image.
- Axis selection is automatic: "Items with an icon... change their axis to be vertical. And text-only items... continue to stay on the horizontal axis." "Custom or complex views stay horizontal by default."
- "Keep text-based buttons to a minimum. Labels that include text stay in a horizontal bar, so prefer a symbol wherever one works." (HIG-DUO) Exceptions that must stay horizontal: items "too wide for the space on the right, like a text button or a segmented control." (TT-DESIGN)
- Decision test for text: "is it simply reinforcing the symbol, or does it carry standalone information? ... if the text does contain meaningful information, like a cart button displaying a dollar amount, it's better to keep the control in the horizontal bar."
- Replace inline counts with badges (iOS 26 badge API) so the item becomes symbol-only.
- The system Edit button already stays horizontal. A custom item that switches between symbol and text (e.g. Select/Done) must be forced horizontal.

```swift
// Set a preferred axis for a custom view  (TT-BARS 8:08)
// SwiftUI
var body: some View {
    ContentView()
        .toolbar {
            ToolbarItem {
                ProfileView()
            }
            .axisBehavior(.verticalPreferred)
        }
}

// UIKit
let item = UIBarButtonItem(customView: ProfileView())
item.axisBehavior = .verticalPreferred
```

```swift
// Keep an item on the horizontal axis  (TT-BARS 8:36)
// SwiftUI
var body: some View {
    ContentView()
        .toolbar {
            ToolbarItem {
                SelectOrDoneButton()
            }
            .axisBehavior(.horizontalOnly)
        }
}

// UIKit
item.axisBehavior = .horizontalOnly
```

```swift
// Use a badge instead of inline text  (TT-BARS 9:27)
// SwiftUI
var body: some View {
    ContentView()
        .toolbar {
            ToolbarItem(...) {
                InboxButton()
                    .badge(7)
            }
        }
}

// UIKit
let item = UIBarButtonItem(...)
item.badge = .count(7)
```

## Item representation rules (PREP, verbatim)

"When you create toolbar items, specify both an icon and a title for items that you want to have the most adaptability. iPhone Duo might present items vertically, horizontally, or in an overflow menu:
- The system uses an icon for an item it presents vertically.
- The system uses an icon or a title for an item it presents horizontally, preferring an icon.
- The system uses an icon and title for an item in an overflow menu.
- If your item has a title and doesn't have an icon, the system doesn't present it vertically.
- If your item uses a custom view rather than a title or icon, the system doesn't present it vertically."

DocC on `ToolbarItemAxisBehavior.horizontalOnly` / `UIBarButtonItem.AxisBehavior.horizontalOnly`: "If an item only supports horizontal bars and no horizontal bars are present, the item is not shown." On `.verticalPreferred`: "If both horizontal & vertical bars are present and the item is `.verticalPreferred`, the system prefers placing the item in the vertical bar."

## Custom views inside a vertical bar (TT-BARS 10:07)

- Custom views must "either fit the bar's fixed width or have a vertically adapted layout." Consider adjusting metrics (Apple's example: an action panel "hides its titles and becomes slightly shorter when vertical").
- Detect a vertical bar by reading `toolbarVerticalEdge` (SwiftUI environment, `HorizontalEdge?`) or `verticalBarEdge` (UIKit trait, `UIVerticalBarEdge`). DocC: it "reflects the system's preferred edge for the vertical bar in the current context, regardless of whether a vertical bar is currently visible"; returns `nil` / `.unspecified` "on devices and in contexts where the system never places a vertical bar." Readable from content views and from an item's custom view.
- "a vertical bar doesn't have a scroll-edge effect by default. However, it does have a background when the reduced transparency accessibility setting is enabled. Make sure your custom view content stays legible regardless."
- Spacing: "flexible spacers are zero size in the vertical axis. But fixed spacers continue to respect their minimum size. Your app shouldn't be creating additional spacing." HIG-DUO: use `ToolbarItemGroup` / `UIBarButtonItemGroup` and "avoid adding fixed spacing yourself."

```swift
// Read the vertical bar edge  (TT-BARS 10:36)
// SwiftUI
struct ContentView: View {
    @Environment(\.toolbarVerticalEdge) var edge

    var body: some View {
        switch edge {
            ...
        }
    }
}

// UIKit
switch traitCollection.verticalBarEdge {
    ...
}
```

## Overflow and compression (HIG-DUO; TT-BARS 11:40–14:21)

- Overflow happens more "on the outer display in landscape... [and] as other competing UI appears, like the keyboard or when using Picture in Picture in open portrait."
- First decision — which bar survives when space is short:
  - Navigation-focused views: toolbar compresses first, tab bar stays. **Default.**
  - Task-oriented views: tab bar compresses (minimizes to a single control) to keep toolbar actions. Mirrors the minimized tab bar on other iPhones.
- Consolidate any custom overflow into the system overflow menu. Reserve the ellipsis symbol for overflow; "give other menus a distinct symbol."
- Visibility priority: "By default, items overflow from bottom to top." Assign `.high` / `.low` / custom priorities, "start giving priority by groups. Then prioritize items within each one." Keep frequently used actions (Compose, New Note) and status-bearing items (badged) visible longest.

```swift
// Configure toolbar compression behavior  (TT-BARS 12:23)
// SwiftUI
var body: some View {
    TabView {
        Tab("Recents", systemImage: "clock") {
            ContentView()
                .toolbarVerticalCompressionBehavior(.prefersToolbarItems)
        }
    }
}

// UIKit
navigationItem.verticalBarCompressionBehavior = .prefersBarItems
```

```swift
// Consolidate actions into the overflow menu  (TT-BARS 12:43)
// SwiftUI
var body: some View {
    ContentView()
        .toolbar {
            ToolbarOverflowMenu {
                Button("Scan") { ... }
                Button("Connect") { ... }
            }
        }
}

// UIKit
navigationItem.additionalOverflowItems = UIDeferredMenuElement({ provider in
    provider(self.persistentOverflowItems())
})
```

```swift
// Set item visibility priority  (TT-BARS 13:21)
// SwiftUI
var body: some View {
    ContentView()
        .toolbar {
            ToolbarItem {
                Button(...) { ... }
            }
            .visibilityPriority(.high)
        }
}

// UIKit
let item = UIBarButtonItem(...)
item.visibilityPriority = .high
```

Published DocC for the priority types (these pages exist): `ToolbarItemVisibilityPriority` has `.automatic`, `.low`, `.high`, `init(lowerThan:)`, `init(higherThan:)`; `UIBarButtonItemVisibilityPriority` has `.standard`, `.low`, `.high`, `init(_:)`, `init(lowerThan:)`, `init(higherThan:)`, `init(rawValue:)`. `ToolbarOverflowMenu` content "is placed into the overflow menu in the navigation bar" and is always in overflow regardless of space. `additionalOverflowItems`: assigning non-nil makes the overflow button appear on the trailing edge; the system also adds items that don't fit.

Compression enums (DocC): SwiftUI `ToolbarVerticalCompressionBehavior` `.automatic` / `.prefersTabBar` / `.prefersToolbarItems`; UIKit `UIVerticalBarCompressionBehavior` `.automatic` / `.prefersBarItems` / `.prefersTabBar`. Default `.automatic`.

## When to opt out (HIG-DUO; TT-BARS 14:21)

"In general, don't override the default bar placement." (HIG-DUO) Apple's two exceptions:
- "a single-page app with a bottom-heavy layout like Calculator" where horizontal bars let content expand;
- "a sheet [that] is control-heavy with only one item, like the close button."

```swift
// Disable the vertical bar  (TT-BARS 14:47)
// SwiftUI
var body: some View {
    NavigationStack {
        ContentView()
            .toolbarVerticalBehavior(.disabled)
    }
}

// UIKit
class MyViewController: UIViewController {
    override var preferredVerticalBarBehavior: UIVerticalBarBehavior {
        .disabled
    }
}
```

When disabled in a sheet on the outer display, "sheets stop just short of the front-facing camera and the status bar repositions itself." (TT-DESIGN)

DocC guidance on `toolbarVerticalBehavior(_:)` / `preferredVerticalBarBehavior`: "Disable the vertical bar only for UIs that are better served by horizontal bars — such as a fullscreen video player with toolbar controls, or a non-scrolling layout like a calculator where horizontal space is at a premium. Treat it as a stable choice: avoid changing it frequently as the user navigates, and don't toggle it for a single view as a function of that view's state. To hide the bars on a given screen rather than change the layout, use `toolbarVisibility(_:for:)` instead." Resolution: a `NavigationStack` "uses the top most view of its stack", a `TabView` "uses the selected view", a `NavigationSplitView` "uses the view in the trailing-most column." "When the value changes, the system animates the transition: content reflows to or from the horizontal bars while the status bar changes axis and the leading or trailing safe area inset for the vertical bar is added or removed." 

## Full-width layouts without bars (HIG-DUO)

"Some layouts can span the full display, which works well for visual, immersive interfaces that don't scroll, as long as nothing conflicts with the Dynamic Island or the status bar. Calculator, for example, occupies the full width of the display. You can also combine both approaches, letting a background image or header span the full width while scrollable content stays inset."
