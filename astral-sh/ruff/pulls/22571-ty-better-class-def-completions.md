```yaml
number: 22571
title: "[ty] Better class def completions"
type: pull_request
state: open
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
base: main
head: better-class-def-completions
created_at: 2026-01-14T14:28:57Z
updated_at: 2026-01-14T14:29:40Z
url: https://github.com/astral-sh/ruff/pull/22571
synced_at: 2026-01-14T14:41:25Z
```

# [ty] Better class def completions

---

_@RasmusNygren_

## Summary

Prefer completions of type `ClassLiteral` inside class-def context.

Fixes https://github.com/astral-sh/ty/issues/1776

## Test Plan
New completion-eval test that has incorrect ranking on main but correct after this patch.


---

_Review requested from @carljm by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @MichaReiser by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @sharkdp by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @dcreager by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @Gankra by @RasmusNygren on 2026-01-14 14:28_

---

_Label `server` added by @AlexWaygood on 2026-01-14 14:29_

---

_Label `ty` added by @AlexWaygood on 2026-01-14 14:29_

---
