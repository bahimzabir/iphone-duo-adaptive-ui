# iPhone Duo Adaptive UI

A skill for AI coding agents (Claude Code, Codex, Cursor, Copilot and others) that teaches them how to build and review SwiftUI and UIKit apps for **iPhone Duo**, Apple's folding iPhone.

Everything in it comes from Apple's own material: the Human Interface Guidelines, Apple's developer articles, the iOS 27.1 SDK and API reference, the six iPhone Duo Tech Talks and the three Group Lab Q&A sessions. Where Apple has not published something, the skill says so and tells the agent to ask you instead of guessing.

Current version: **0.3.0** (October 26, 2026). iPhone Duo has been on sale since October 23, 2026, running iOS 27.1.

## Install

Pick the one that matches your setup.

**Any agent** (Claude Code, Codex, Cursor, Copilot, Gemini CLI, Windsurf, Cline and more). Uses the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add bahimzabir/iphone-duo-adaptive-ui
```

Add `--list` to see what you get first, or `-g -a claude-code -y` to install globally for Claude Code without prompts.

**Claude Code plugin:**

```
/plugin marketplace add bahimzabir/iphone-duo-adaptive-ui
/plugin install iphone-duo-adaptive-ui@ios-skills
```

**By hand**, for any agent that reads `SKILL.md` folders:

```bash
git clone https://github.com/bahimzabir/iphone-duo-adaptive-ui.git
ln -s "$(pwd)/iphone-duo-adaptive-ui/skills/iphone-duo-adaptive-ui" ~/.claude/skills/iphone-duo-adaptive-ui
```

Use `.claude/skills/` inside a project instead of `~/.claude/skills/` to install for one project only.

**claude.ai:** zip the `skills/iphone-duo-adaptive-ui` folder and upload it under Settings → Features. Needs a plan with code execution.

**Claude API:**

```python
from anthropic import Anthropic
from anthropic.lib import files_from_dir

client = Anthropic()
skill = client.skills.create(files=files_from_dir("skills/iphone-duo-adaptive-ui"))
```

Once installed, type `/iphone-duo-adaptive-ui` or just ask for something like "get my app ready for iPhone Duo". The agent picks the skill up on its own when a task mentions iPhone Duo, the fold, vertical bars, `ArrangementView`, and so on.

## What the agent learns

- **Layout on size classes, never on device.** No `UIScreen.main`, no idiom or orientation checks, no hard-coded iPhone sizes. Both of Apple's lists of patterns to remove are included.
- **Vertical bars.** On the outer display and in inner landscape, toolbars and tab bars move to the side. How items need titles and symbols, how overflow and priority work, when to opt out.
- **The fold.** `ArrangementView` and `UIArrangementViewController`, reserved regions, when content should and should not move away from the hinge.
- **Hinge, scenes, camera.** `UIHingeInteraction` and `onHingeChange` for effects only, Split View multitasking, scene accessories on the outer display, camera direction.
- **Shipping.** Which SDK gives you what on the device, how to test (Device Hub, iPhone Mirroring, the Duo simulator), App Store screenshot sizes and the April 2027 screenshot requirement.
- **A review checklist** the agent runs over an existing app, with every item tied to an Apple statement.
- **How to use Apple's own skill.** Xcode 27.1 ships an `app-resizability` skill that rewrites old `UIScreen.main`, orientation and idiom code. This skill tells the agent to run it first and covers everything Apple's skill does not.

## What the agent will not make up

Apple has not published point dimensions, the display scale factor, the width of the vertical bar, hinge angle thresholds or safe-area values. Apple's two published inner-display pixel figures even disagree (tech specs: 1878 × 2670; App Store Connect: 2007 × 2853). The skill records both and refuses to derive anything from them. Same for web, React Native, Flutter and Unity: Apple has no iPhone Duo guidance for them, so the skill stops and asks.

A few iOS 27.1 symbols still have no documentation page (`onHingeChange`, `UIBarButtonItem.badge`, `UICornerConfiguration`, the outer and inner camera device types). They were checked against the released SDK and are marked as such, so the agent tells you when it uses one.

## Sources

Fetched September 10, 2026, re-checked September 22 and October 26.

- HIG: *Designing for iPhone Duo*, *Layout*
- Apple article: *Preparing your app for iPhone Duo*
- developer.apple.com/iphone-duo: the hub page and *Three steps to make your app shine on iPhone Duo*
- Apple news (October 5, 2026) and App Store Connect screenshot and app-preview specifications
- iOS 27.1 API reference, Xcode 27.1 release notes, the iOS 27.1 SDK itself
- Six Tech Talks and three Group Labs (transcribed from Apple's subtitle tracks)
- Apple's `app-resizability` skill, exported from Xcode 27.1 (summarized, not copied)
- apple.com tech specs, for pixel resolution only

Every URL, fetch date and dead link is in `skills/iphone-duo-adaptive-ui/references/sources.md`. Four Developer Forums Q&A threads linked from Apple's hub could not be fetched by a script; their URLs are listed there too.

Nothing has been run on real hardware. All sample code is Apple's, reproduced verbatim.

## Files

```
skills/iphone-duo-adaptive-ui/
  SKILL.md                                    rules, fast facts, 10-step workflow, anti-patterns
  references/
    hig-designing-for-iphone-duo.md           Apple's HIG page, full text
    apple-preparing-your-app-for-iphone-duo.md  Apple's article, full text
    apple-get-ready-prepare.md                Apple's "Three steps" page, news, App Store specs
    apple-app-resizability-skill.md           how to get and use Apple's Xcode skill
    device-facts.md                           anatomy, size classes, SDK behavior, what is not documented
    design-principles.md                      displacement, consistency, outer and inner display patterns
    vertical-bars-and-toolbars.md             side bars and toolbar items, with code
    arrangement-views-and-reserved-regions.md ArrangementView, reserved regions, with code
    hinge-multitasking-scenes.md              hinge, Split View, scenes, camera
    group-lab-qa.md                           Apple panel Q&A from the three Group Labs
    api-index.md                              every symbol, its source and documentation status
    audit-checklist.md                        review checklist
    sources.md                                URLs and fetch dates
.claude-plugin/                               Claude Code plugin and marketplace manifests
```

## Changelog

**0.3.0** (2026-10-26). iOS 27.1 and Xcode 27.1 are released; beta wording removed. Added Apple's "Three steps" page, the October 5 news, App Store asset specs, all three Group Lab transcripts and a guide to Apple's `app-resizability` skill. Symbols without a documentation page verified against the SDK.

**0.2.0** (2026-09-22). Apple's article "Preparing your app for iPhone Duo" and the iOS 27.1 beta API reference incorporated.

**0.1.0** (2026-09-10). First version, from the HIG page and the six Tech Talks published the day before.

## Keeping it current

Re-check when any of these happen:

- A new iOS or Xcode 27.x ships. Compile the code samples in `references/`, run the Duo simulator through open, close, rotate, fold and Split View on both sides.
- The HIG page gets a new change-log entry. Re-fetch it and regenerate `references/hig-designing-for-iphone-duo.md`.
- A symbol marked "SDK-verified, no documentation page" in `references/api-index.md` gets a page. Flip its status and link it.
- Apple publishes point dimensions or a scale factor, or reconciles its two pixel figures. Move them out of the "not documented" list in `references/device-facts.md`.
- The Developer Forums Q&A threads become readable. Add them to `references/group-lab-qa.md`.

Issues and pull requests welcome.

## License

Skill text: MIT (see `LICENSE`). Quoted Apple documentation remains © Apple Inc. and is not relicensed.
