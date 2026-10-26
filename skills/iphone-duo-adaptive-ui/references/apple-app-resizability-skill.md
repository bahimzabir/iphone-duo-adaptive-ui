# Apple's own agent skill: `app-resizability` (ships inside Xcode 27.1)

Source ID: **XC-SKILL**. Apple's "Three steps" page (`apple-get-ready-prepare.md` §2, Step 1) tells developers to run the Xcode coding assistant's app-resizability skill and, for other agents, to export it with `xcrun agent skills export`. Inspected **2026-10-26** from Xcode 27.1 (27A9275). The exported files carry no license file, so this skill does **not** vendor them; it tells you how to get them and summarizes what they cover, with short attributed quotes.

## How to get it

```bash
# Xcode 27.1 required. The output directory must be an absolute path.
xcrun agent skills export --output-dir "$PWD/apple-skills" --replace-existing
```

Exports ten skills; the Duo-relevant one is `apple-skills/app-resizability/`. Install it alongside this skill (for Claude Code: copy or symlink the folder into `~/.claude/skills/`). Its frontmatter: "Use when an app's layout must adapt to a window that changes size while the app runs... preparing or optimizing an app for the foldable iPhone Duo, split-screen multitasking, or Stage Manager."

**Division of labor.** Apple's skill is a *code-modernization* workflow: it detects and rewrites `UIScreen.main`, orientation, idiom, safe-area and app-lifecycle code in UIKit (Swift and Objective-C) with a SwiftUI section per task. This skill (`iphone-duo-adaptive-ui`) covers what Apple's does not: design guidance, vertical bars and toolbar items, `ArrangementView`, reserved regions, hinge, scenes, camera, and the audit checklist. When a task is "get my app ready for iPhone Duo", run Apple's skill first for the mechanical fixes, then this one.

## What it contains (file → scope)

| File | Apple's description |
|------|---------------------|
| `SKILL.md` | 14 core principles, Info.plist prerequisites, 5-phase workflow, task registry. "Treat a request about the foldable iPhone Duo as a request for every task in the Task Registry, because a screen that changes size exposes all of them at once." |
| `references/uiscreen-task.md` | "Replace `UIScreen.main` with context-appropriate APIs" (scale → `traitCollection.displayScale` with trait-change invalidation; bounds → `view.bounds`; `nativeScale` has no trait-collection equivalent; keyboard notification coordinate space; Catalyst; `UIWindow(windowScene:)`). |
| `references/orientation-task.md` | "Replace layout-related orientation checks with size classes or window bounds, and migrate the deprecated scene geometry callback." |
| `references/scene-lifecycle-task.md` | "Migrate AppDelegate to SceneDelegate." |
| `references/safe-area-task.md` | "Replace `topLayoutGuide` and `bottomLayoutGuide`, replace hard coded values for insets with safe area references, and ensure that existing references work with asymmetric safe areas and with insets that change while the app runs." |
| `references/idiom-task.md` | "Replace layout-related idiom checks with size classes, and read a genuinely needed idiom from the local trait collection." |

## Prerequisite checks Apple's skill runs (read-only, once per project)

| Check | Satisfied when | Otherwise Apple says |
|-------|----------------|----------------------|
| Launch screen | Target sets `UILaunchScreen`, `UILaunchScreens`, `UILaunchStoryboardName`, `UILaunchStoryboards`, or `INFOPLIST_KEY_UILaunchScreen_Generation` | "The app declares no launch screen. iOS 27 rejects the App Store upload with `ITMS-90870`. Refer to TN3208." |
| iPad orientations | The iPad declaration lists all four orientations, or none | "Name the orientations the declaration omits." |
| Full screen opt-out | `UIRequiresFullScreen` absent, or `UIRequiresFullScreenIgnoredStartingWithVersion` set | "The target sets `UIRequiresFullScreen`. iOS 27 ignores it and resizes the scene anyway. Refer to TN3192." — "Never delete `UIRequiresFullScreen`, because deletion makes the app resizable at once and the layouts may not be ready. Never add `UIRequiresFullScreenIgnoredStartingWithVersion`, because its value decides which releases keep the old behavior, and that is the developer's decision." |

## Statements worth carrying into Duo reviews (quoted)

- Orientation: "`UIWindowScene.Geometry.interfaceOrientation`, reached as `windowScene.effectiveGeometry.interfaceOrientation`, is current API. Being current does not put it out of scope. A layout decision that reads it is a detection target." — "Interface orientation describes how the UI is rotated relative to the device reference. It does not encode an aspect ratio." Orientation locking: "the modern API is `prefersInterfaceOrientationLocked` (iOS 26+)" (confirmed in `UIViewController.h`, iOS 27.1 SDK).
- Idiom: "`userInterfaceIdiom` encodes a device class, not the space a view actually has." — "The mapping is not one to one, and that is the point. An iPad in a narrow Split View reports `.compact` horizontally, and a large iPhone in landscape reports `.regular`." — "Pick the axis the code is actually about."
- Safe areas: "Asymmetric horizontal insets are the normal case. Where a vertical bar is present, one horizontal edge usually carries the whole inset and the other carries zero. Which edge carries it changes with the orientation and with the display in use." — "The inset is also much larger than the horizontal insets of earlier devices." — "Do NOT test an inset against a threshold to decide whether a safe area exists." — "**Subtracting both horizontal insets from one total width is correct.**"
- Which edge holds the bar: "Read `traitCollection.verticalBarEdge` (iOS 27.1). A read inside a layout method needs nothing else... A read anywhere else needs a registration in the same edit":

  ```swift
  registerForTraitChanges(UITraitCollection.systemTraitsAffectingVerticalBarEdge) { (self: Self, _) in
      self.view.setNeedsLayout()
  }
  ```

  "It resolves to `leading` or `trailing`, which follow layout direction, while `safeAreaInsets` uses physical edges. And `unspecified` is ambiguous... Read the inset when you need the size of the space." (`systemTraitsAffectingVerticalBarEdge` confirmed in `UIVerticalBarEdge.h`, iOS 27.1 SDK.)
- Physical vs directional: "`safeAreaInsets` describes the physical enclosure. `.left` is the physical left in both layout directions, and it never means *leading*." Constrain to `safeAreaLayoutGuide.leadingAnchor` / `.trailingAnchor` when the code means a direction.
- Never store an inset; read insets "from the view that lays out"; use `keyboardLayoutGuide` for keyboard avoidance.
- Core principle 11: "Never walk global scene/window state — Never use `UIApplication.shared`, `UIDevice.current`, `UIScreen.main`, or other shared objects as a replacement."

## What Apple's skill does not cover (use this skill)

Vertical-bar item design and APIs (`axisBehavior`, `visibilityPriority`, compression, opt-out), `ArrangementView` / `UIArrangementViewController`, reserved regions, hinge APIs, scene accessories and camera direction, HIG design rules, games, App Store assets. It also contains no device numbers; the "NOT documented" list in `device-facts.md` still stands.
