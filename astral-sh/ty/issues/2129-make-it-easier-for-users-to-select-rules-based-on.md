```yaml
number: 2129
title: Make it easier for users to select rules based on strictness (profiles, rule categories)
type: issue
state: open
author: OEvortex
labels:
  - server
assignees: []
created_at: 2025-12-20T11:31:03Z
updated_at: 2025-12-31T16:56:59Z
url: https://github.com/astral-sh/ty/issues/2129
synced_at: 2026-01-12T15:54:26Z
```

# Make it easier for users to select rules based on strictness (profiles, rule categories)

---

_@OEvortex_

I’d like a per-project (or workspace) toggle to enable or disable Python type checking similar to Pylance’s "Type Checking Mode" (off/basic/strict). This should let users quickly switch type checking behavior without changing global settings or editing config files.

Motivation
- Some projects rely heavily on type checking while others (or legacy codebases) are noisy; switching globally is disruptive.
- Pylance’s UI makes it easy to change modes per-workspace; having the same capability would improve developer experience and reduce friction when working across multiple repositories.

Proposed behavior
- Expose a workspace-level setting named something like "python.typeCheckingMode" with values: "off", "basic", "strict".
- Provide a command palette action (e.g., “Toggle Python Type Checking”) that cycles modes or opens a quick pick with the three options.
- Persist the chosen mode in workspace settings (and allow user-level override).
- When "off" is selected, the language server/extension must suppress type-check diagnostics entirely.
- When "basic" or "strict" are selected, diagnostics should behave accordingly (match existing behavior used for current strictness levels).
- Changing the mode should immediately re-run analysis and update diagnostics without requiring restart.

UI/UX
- Quick pick command accessible from Command Palette.
- Optional status bar item showing current mode; clicking it opens the quick pick.
- Setting visible in workspace settings UI.

Implementation notes (suggested)
- Reuse existing type-checking diagnostic pipeline; gate diagnostics emission based on the workspace setting.
- Ensure workspace-level configuration overrides user-level defaults.
- Emit telemetry (opt-in) for mode changes to understand usage patterns.
- Add unit and integration tests to validate persistence and immediate refresh behavior.

Acceptance criteria
- Workspace setting exists and accepts "off", "basic", "strict".
- Command palette item and status bar entry present and functional.
- Diagnostics update immediately when toggling modes.
- Workspace-level setting overrides user-level setting.


---

_Label `server` added by @carljm on 2025-12-20 16:21_

---

_Comment by @MichaReiser on 2025-12-20 17:07_

https://github.com/astral-sh/ruff/pull/22073 adds support for turning diagnostics off. 

We agree that we need an easier way to switch between type checker strictness. But we don't know yet if that's going to be rule categorize or profiles, similar to pyright

---

_Added to milestone `M1` by @MichaReiser on 2025-12-20 17:07_

---

_Removed from milestone `M1` by @MichaReiser on 2025-12-20 17:07_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-20 17:07_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:35_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:35_

---

_Renamed from "Add per-project toggle to enable/disable type checking (like Pylance)" to "Make it easier for users to select rules based on strictness (profiles, rule categories)" by @MichaReiser on 2025-12-31 16:56_

---
