# Design principles and layout patterns for iPhone Duo

Distilled from HIG-DUO, TT-DESIGN, TT-PREPARE, and TT-POSE (see `sources.md`). Quotations are verbatim Apple text; everything else paraphrases Apple text without adding claims.

## 1. One app, two size classes — not one layout per pose

- "Supporting the device's various poses doesn't mean designing a custom layout for each one: instead, use Size classes so your app adapts naturally as it changes size." (HIG-DUO)
- "Don't reinvent your app when it resizes; allow the existing layout to expand based on the available space instead." (HIG-DUO)
- "Avoid fixed widths, breakpoints, or any metrics tied to a specific screen. Instead, always target these size classes." (TT-DESIGN)
- "Design your app to be freely resizable and you'll be in good shape." (TT-DESIGN)
- "Avoid making assumptions about display sizes or device capabilities based on user interface idioms." (TT-PREPARE)
- Optional exception — the table/laptop pose: "Media can sit at the top, while tappable controls live on a stable base at the bottom. If you decide to do a special layout for this pose, just be sure it has all the same controls and general hierarchy as other poses. You don't want to tie functionality to a specific pose." (TT-DESIGN)

## 2. Consistency across displays and poses

- "Keep functionality and the state of elements the same between displays. Maintain your app's information hierarchy, but show an additional level of hierarchy on the larger inner display if it makes sense for your content." Example: Mail shows list *or* message on the outer display, both side by side on the inner display. (HIG-DUO)
- "Provide access to the same controls and content regardless of how someone holds or views the device." (HIG-DUO)
- "People may open and close the device frequently while using your app, so it needs to be predictable and consistent inside and out." (TT-DESIGN)

## 3. Build on layout margins and safe areas; expect asymmetry

- "As with all iOS devices, build your layouts with layout margins and safe area insets, and steer clear of fixed widths or anything tied to a specific display." (HIG-DUO)
- Foreground vs background: "place foreground elements like interactive controls within the safe area... Allow background elements such as full-bleed artwork to fill the available space, extending behind toolbars and sidebars." (TT-PREPARE)
- "Keep in mind that safe areas are often asymmetric. This is especially true on iPhone Duo. For example, vertical buttons can appear on the left side in landscape and Split View multitasking. As such, avoid assuming that insets on opposite sides are equal. Instead, write code that handles each side independently." (TT-PREPARE)
- "As with safe areas, layout margins are also asymmetric. This lets your foreground content get closer to vertical buttons and the status bar, while preserving the margin on the opposite side." (TT-PREPARE)
- "Use safe areas to make sure controls don't cover your content, including controls on the opposite edge, like when two apps share the inner display with Split View multitasking." (HIG-DUO)

## 4. The outer display: content is offset, not centered (usually)

- "On iPhone Duo, most content needs an offset so it isn't hidden behind the controls. If your app aligns to horizontal safe area insets, that offset happens automatically." (TT-DESIGN)
- Full-width exception: "Some UI should still center on the full display though, no offset. That works well for immersive, highly visual interfaces that don't scroll, as long as you're sure interactive elements won't be blocked by controls on the right." Calculator is Apple's example: on iPhone 16 it uses four columns of five buttons; on the Duo outer display, five columns of four. (TT-DESIGN; HIG-DUO)
- Mixed approach: "a full-width background image or header with scrollable foreground content that's inset. Just to make sure every interactive element lives inside that scrollable area so nothing gets covered up." (TT-DESIGN)

## 5. The inner display: use the width, don't stretch

- "You don't want just a stretched out iPhone app." Three Apple-listed options (TT-DESIGN):
  1. **Split views** — "surface multiple levels of your app hierarchy at the same time." Hierarchy must not change between outer and inner displays.
  2. **Two-column rearrangement** — Music example: "a vertically stacked layout that rearranges itself into a two-column layout when there's more horizontal space."
  3. **Tab bar as sidebar** — "present your tab bar as a sidebar on the inner display. Now this isn't for every app. It works best for information-dense apps like this example from Health."
- "Take advantage of the wide inner display to show more content." (TT-DESIGN chapter 7:34)

## 6. The fold: displacement, not rearrangement

From HIG-DUO "Reserved regions" and TT-POSE.

- The folding region "is conditional based on how a person uses the device. When the device is partially open, the folding region divides the inner display into multiple usable regions, excluding the region at the center as the display folds." (HIG-DUO)
- Analogy: "Just as a photo spread across a book's spine stops reading as one continuous image, content and controls that span the fold become harder to see." (TT-POSE 1:29)
- "Prefer a layout container that adapts automatically, like the split view in Notes that adjusts the width of each pane to stay clearly visible as the device folds. In a grid-style layout, prefer an even number of columns so content divides cleanly." (HIG-DUO)
- "Avoid extreme layout changes as people fold the device. Move only what's necessary to keep elements visible and easy to tap. Controls that disappear or shift dramatically are harder to find and track, so favor small adjustments over rearrangement." (HIG-DUO)
- **Displacement pattern** (TT-POSE 2:26): "adjusting the frame of existing elements based on available space. Displacement can scope from a single button to an entire container. Move elements independently when they can adapt alone, together when they work as a unit, and avoid excessive movement that weakens visual relationships. Continuously scrolling content like articles and feeds shouldn't displace."
- **Where content moves** (TT-POSE 4:00): "Partially folded like a book, alerts move to the trailing side, closer to where they'll appear as the device closes. Propped on a table, the top region suits content viewed at a distance while the bottom suits interactive controls."
- **What adapts automatically** (HIG-DUO; TT-DESIGN 9:28; TT-POSE 5:12): alerts, action sheets, context menus, menus, popovers, sheets, toolbar buttons, split-view columns ("keeps both columns visible with an even split"), and grids that "preserve outer margins while increasing spacing around the hinge." Apple's phrasing: "a behavior that nudges interactive elements away from the center when iPhone Duo is partially folded... built into a bunch of system components." Scrollable content is exempt: "Scrollable content doesn't need to avoid this region." (TT-DESIGN)
- For custom UI: "Use the reserved region APIs to keep important elements clear of the center if the system doesn't move them automatically." (HIG-DUO) See `arrangement-views-and-reserved-regions.md`.

## 7. Sheets and presentations by pose (TT-DESIGN 8:36; TT-PREPARE)

- Outer display: "sheet controls also move to the side. But you can keep them from moving if that fits your content better by disabling the vertical bar in the sheet. This works well for sheets with only a single toolbar button. If you use this option, sheets stop just short of the front-facing camera and the status bar repositions itself."
- Inner display, both orientations: "sheets use standard horizontal bars" and "sheets are centered."
- Partially folded: "sheets slide over to avoid resting in the fold."
- "Other presentations like popovers, context menus, and alerts also adapt to each pose." (TT-PREPARE)

## 8. Games (HIG-DUO "Best practices")

"Make your game playable in every device pose. You can choose to lock to either portrait or landscape orientation, but be sure to fill the screen as the device pose changes. When resizing, keep text and control sizes as consistent as possible. Prefer changing the aspect ratio over letterboxing or pillarboxing in games; if you can't avoid letterboxing or pillarboxing, add artwork to the padding area to help the experience feel full screen." No further Duo-specific game guidance exists as of 2026-09-10.

## 9. The one-sentence goal

"Your app should feel like a single experience that adapts different display sizes and poses. By supporting resizability, your app will feel at home on iPhone Duo." (TT-DESIGN)
