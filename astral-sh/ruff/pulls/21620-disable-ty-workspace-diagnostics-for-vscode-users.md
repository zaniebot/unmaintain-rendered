```yaml
number: 21620
title: Disable ty workspace diagnostics for VSCode users
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/ty-workspace-settings
created_at: 2025-11-24T19:49:56Z
updated_at: 2025-11-24T20:06:12Z
url: https://github.com/astral-sh/ruff/pull/21620
synced_at: 2026-01-12T15:57:29Z
```

# Disable ty workspace diagnostics for VSCode users

---

_@AlexWaygood_

## Summary

It's currently pretty painful to have ty's workspace diagnostics enabled in the Ruff repo, because ty will proceed to type-check every single fixture in the `ruff_linter` crate, resulting in >10,000 diagnostics. This PR disables ty workspace diagnostics if you're browsing this repo inside VSCode.

## Test Plan

On this branch, I no longer have >10,000 diagnostics reported in the `problems` tab in VSCode, despite having the ty-vscode plugin installed and despite having this setting in my VSCode user settings:

```json
    "ty.diagnosticMode": "workspace"
```

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-24 19:49_

---

_Label `internal` added by @AlexWaygood on 2025-11-24 19:49_

---

_@amyreese approved on 2025-11-24 19:52_

---

_Comment by @Gankra on 2025-11-24 19:53_

To be clear you specifically have workspace diagnostics custom-enabled in your vscode, and this is "and it sucks that i did, in this project in particular"?

---

_Comment by @AlexWaygood on 2025-11-24 20:02_

> To be clear you specifically have workspace diagnostics custom-enabled in your vscode, and this is "and it sucks that i did, in this project in particular"?

yeah, kinda:
1. I want workspace diagnostics enabled for all my Python projects, so I enabled that in my user settings, and it was great, I love it
2. But then I opened up Ruff and hoo boy, I cannot deal with that many diagnostics in the problem tab
3. So I went to disable it in the workspace settings for Ruff, and that was nice, now I have workspace diagnostics enabled for all my Python projects but not for this one special project (Ruff) that isn't really a Python project but happens to have literally thousands of lines of Python that happen to be deliberately of questionable quality
4. But oh no, now I can't do `git commit -am` anymore because Ruff apparently commits `.vscode/settings.json` into source control for some reason rather than `.gitignor`ing it. So PR'ing it up (as I'm doing here) is the only solution here, it seems?

---

_Comment by @Gankra on 2025-11-24 20:04_

> but happens to have literally thousands of lines of Python that happen to be deliberately of questionable quality

Well that seems as good a reason as any, ship it

---

_Merged by @AlexWaygood on 2025-11-24 20:06_

---

_Closed by @AlexWaygood on 2025-11-24 20:06_

---

_Branch deleted on 2025-11-24 20:06_

---
