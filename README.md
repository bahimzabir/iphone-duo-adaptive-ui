# iphone-duo-adaptive-ui

An Agent Skill that teaches AI coding agents how to build and review SwiftUI and UIKit interfaces for **iPhone Duo**, Apple's folding iPhone (announced September 2026, iOS 27.1).

> ## ⚠️ Beta-stage guidance — Apple's docs are published, the SDK is beta
>
> **v0.2.0 (2026-09-22).** First assembled on 2026-09-10, one day after Apple published the iPhone Duo HIG page and six Tech Talks, when no SDK and no API reference existed. Re-verified on 2026-09-22 after Apple shipped **Xcode 27.1 beta** (developer email of 2026-09-18) and published the article **"Preparing your app for iPhone Duo"** plus the DocC reference pages. Current state:
>
> - **Published and incorporated:** the article (full text in `references/`), DocC pages for `ArrangementView`, `UIArrangementViewController`, `ReservedRegion` / `UIView.ReservedRegion`, `toolbarVerticalBehavior`, `toolbarVerticalEdge`, compression behaviors, `axisBehavior`, `presentationPlacement`, `UIHinge` / `UIHingeInteraction`, scene accessories, `AVCaptureDeviceDirectionCoordinator`, and the Xcode 27.1 beta release notes. All carry availability **iOS 27.1 beta**.
> - **Still unpublished:** the SwiftUI `onHingeChange` modifier, `UIBarButtonItem.badge`, iOS 27.1 release notes. These remain Tech-Talk-only and are marked as such in `references/api-index.md`.
> - **Still not stated by Apple anywhere:** point dimensions, display scale factor, vertical-bar width, hinge angle thresholds, safe-area values. The skill lists these as "not documented" and instructs agents to ask rather than guess.
> - **Beta means names can move.** Re-run the Maintenance steps when iOS 27.1 goes GA.
> - **Nothing here has been run on hardware.** Sample code is Apple's, reproduced verbatim; the iPhone Duo Simulator has known gaps (slow first launch, no StandBy, most app extensions unavailable).

## What it is grounded in

Only Apple sources, fetched 2026-09-10 and re-verified 2026-09-22:

- Human Interface Guidelines — *Designing for iPhone Duo* (new page dated September 9, 2026; unchanged as of September 22)
- Technology Overviews — *Preparing your app for iPhone Duo* (published September 2026)
- DocC reference pages for the iOS 27.1 beta APIs and the Xcode 27.1 Beta Release Notes
- Human Interface Guidelines — *Layout* (size classes, safe areas)
- Six Apple Tech Talks: *Design for iPhone Duo*, *Prepare your app for iPhone Duo*, *Raise the bar with iPhone Duo*, *Strike a pose with adaptive layouts on iPhone Duo*, *Leverage multiple displays and scenes on iPhone Duo*, *Build a great camera experience for iPhone Duo* (chapter summaries, on-page sample code, and English subtitle transcripts)
- apple.com tech specs (pixel resolution only, clearly labelled)

Full list with URLs: `skills/iphone-duo-adaptive-ui/references/sources.md`.

The skill deliberately lists what Apple has **not** published (point sizes, scale factor, bar width, hinge thresholds, non-native guidance) so agents ask instead of guessing.

## Layout

```
.claude-plugin/
  plugin.json                              Claude Code plugin manifest
  marketplace.json                         this repo is its own Claude Code marketplace ("ios-skills")
skills/
  iphone-duo-adaptive-ui/                  the skill (folder name == frontmatter name, per Agent Skills spec)
    SKILL.md                               entry point: rules, fast facts, 10-step workflow, anti-patterns
    references/
      hig-designing-for-iphone-duo.md      Apple's HIG page, full text
      apple-preparing-your-app-for-iphone-duo.md  Apple's developer article, full text
      device-facts.md                      anatomy, size classes, SDK gating, NOT-documented list
      design-principles.md                 displacement, consistency, outer/inner patterns, sheets, games
      vertical-bars-and-toolbars.md        side bars, item axis, overflow, priority, opt-out (+ code)
      arrangement-views-and-reserved-regions.md  ArrangementView, split/overlay, reserved regions (+ code)
      hinge-multitasking-scenes.md         hinge API, Split View, scenes, accessories, camera direction
      api-index.md                         every symbol with source, timestamp, DocC status
      audit-checklist.md                   pass/fail review list
      sources.md                           URLs, fetch date, 404 list
LICENSE                                    MIT for the skill text; Apple content notice
```

## Install

Repository: https://github.com/bahimzabir/iphone-duo-adaptive-ui

**Any agent — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Windsurf, Cline, and 70+ more** (uses the [`skills`](https://github.com/vercel-labs/skills) CLI, follows the [Agent Skills](https://agentskills.io) spec):

```bash
# interactive: pick agents, project or global scope
npx skills add bahimzabir/iphone-duo-adaptive-ui

# non-interactive, global, Claude Code only
npx skills add bahimzabir/iphone-duo-adaptive-ui --skill iphone-duo-adaptive-ui -g -a claude-code -y

# see what the repo offers without installing
npx skills add bahimzabir/iphone-duo-adaptive-ui --list
```

**Claude Code, as a plugin** (this repo is its own marketplace):

```
/plugin marketplace add bahimzabir/iphone-duo-adaptive-ui
/plugin install iphone-duo-adaptive-ui@ios-skills
```

**Manual** (any agent that reads `SKILL.md` folders):

```bash
git clone https://github.com/bahimzabir/iphone-duo-adaptive-ui.git
ln -s "$(pwd)/iphone-duo-adaptive-ui/skills/iphone-duo-adaptive-ui" ~/.claude/skills/iphone-duo-adaptive-ui   # or .claude/skills/ inside a project
```

**claude.ai**: zip the `skills/iphone-duo-adaptive-ui` folder and upload it under Settings → Features (Pro, Max, Team, Enterprise with code execution enabled).

**Claude API**:

```python
from anthropic import Anthropic
from anthropic.lib import files_from_dir

client = Anthropic()
skill = client.skills.create(files=files_from_dir("skills/iphone-duo-adaptive-ui"))
```

Once installed, invoke it by name (`/iphone-duo-adaptive-ui`, or `/iphone-duo-adaptive-ui:iphone-duo-adaptive-ui` via the plugin route) or let the agent select it from the trigger terms in the `description` frontmatter.

## Maintenance

Re-verify when any of these ship:

1. **Xcode 27.1 / iOS 27.1 SDK** — compile every code sample in `skills/iphone-duo-adaptive-ui/references/`; run the iPhone Duo simulator in Device Hub through open / close / rotate / fold and Split View on both sides; fix any symbol whose spelling changed.
2. **Apple's article "Preparing your app for iPhone Duo"** — diff it against `skills/iphone-duo-adaptive-ui/SKILL.md` and `skills/iphone-duo-adaptive-ui/references/design-principles.md`; add anything new, remove anything contradicted.
3. **DocC pages for the iOS 27.1 APIs** — re-probe every ❌ row in `skills/iphone-duo-adaptive-ui/references/api-index.md` (endpoint pattern: `https://developer.apple.com/tutorials/data/documentation/<path>.json`), flip the status column, and link the pages.
4. **HIG "Designing for iPhone Duo" change log** — if a new dated entry appears, re-fetch the page (`https://developer.apple.com/tutorials/data/design/human-interface-guidelines/designing-for-iphone-duo.json`) and regenerate `skills/iphone-duo-adaptive-ui/references/hig-designing-for-iphone-duo.md`.
5. **Device specifications** — if Apple publishes point dimensions or scale factors in developer documentation, move them from the "NOT documented" list in `skills/iphone-duo-adaptive-ui/references/device-facts.md` into the facts table with the source.

`skills/iphone-duo-adaptive-ui/references/sources.md` records every URL, the fetch date, and the full 404 list from 2026-09-10.

## License

Skill text and structure: MIT (see `LICENSE`). The reference files quote Apple's Human Interface Guidelines and Tech Talks verbatim for reference and commentary; that content is © Apple Inc. and is not relicensed here.
