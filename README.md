# iphone-duo-adaptive-ui

An Agent Skill that teaches AI coding agents how to build and review SwiftUI and UIKit interfaces for **iPhone Duo**, Apple's folding iPhone (announced September 2026, iOS 27.1).

> ## ⚠️ Early release — Apple has not shipped the full docs or SDK yet
>
> This skill was assembled on **2026-09-10**, one day after Apple published the iPhone Duo Human Interface Guidelines page and six Tech Talks, and **before** the tooling and reference documentation were available. As of that date:
>
> - **Xcode 27.1 beta and the iOS 27.1 SDK were not released.** Apple's "Get Ready for iPhone Duo" page listed them as "Coming later this month." None of the code in this skill has been compiled against a shipping SDK.
> - **Apple's documentation article "Preparing your app for iPhone Duo" was not published** (also "Coming later this month").
> - **No DocC reference pages exist for the new iOS 27.1 APIs.** `ArrangementView`, `UIArrangementViewController`, `reservedRegions`, `toolbarVerticalBehavior`, `axisBehavior`, `toolbarVerticalEdge`, `onHingeChange`, `UIHingeInteraction`, scene accessories, `CameraCaptureAccessory`, `AVCaptureDeviceDirectionCoordinator` and others returned HTTP 404. Their spellings come **only** from Apple's Tech Talk sample code and narration and may change before release.
> - **No iPhone Duo simulator was available**, so nothing here has been visually verified in Device Hub.
> - **Apple has not published point dimensions, display scale factor, vertical-bar width, hinge angle thresholds, or safe-area values.** The skill lists these as "not documented" and instructs agents to ask rather than guess.
>
> What this means for you:
>
> 1. Treat every iOS 27.1 symbol as **pre-release**. `skills/iphone-duo-adaptive-ui/references/api-index.md` marks each one with its source, timestamp, and DocC status so you can re-check it.
> 2. Expect to **re-validate the whole skill** once Xcode 27.1 ships. The "Maintenance" section below lists what to re-fetch.
> 3. Design guidance (size classes, vertical bars, displacement, split vs overlay, safe-area asymmetry) comes from the HIG page and Apple designers' talks and is unlikely to change; API spellings are the part most likely to move.
> 4. If a task needs a number Apple has not published, **stop and ask** — the skill's ground rules forbid inventing one.

## What it is grounded in

Only Apple sources, fetched 2026-09-10:

- Human Interface Guidelines — *Designing for iPhone Duo* (new page dated September 9, 2026)
- Human Interface Guidelines — *Layout* (size classes, safe areas)
- Six Apple Tech Talks: *Design for iPhone Duo*, *Prepare your app for iPhone Duo*, *Raise the bar with iPhone Duo*, *Strike a pose with adaptive layouts on iPhone Duo*, *Leverage multiple displays and scenes on iPhone Duo*, *Build a great camera experience for iPhone Duo* (chapter summaries, on-page sample code, and English subtitle transcripts)
- Existing DocC pages for toolbar priority / overflow APIs
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
