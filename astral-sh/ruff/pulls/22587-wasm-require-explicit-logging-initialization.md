```yaml
number: 22587
title: "wasm: Require explicit logging initialization"
type: pull_request
state: open
author: Jkhall81
labels:
  - playground
assignees: []
base: main
head: fix/wasm-logging-init-22535
created_at: 2026-01-14T22:33:02Z
updated_at: 2026-01-15T08:40:39Z
url: https://github.com/astral-sh/ruff/pull/22587
synced_at: 2026-01-15T08:47:17Z
```

# wasm: Require explicit logging initialization

---

_@Jkhall81_

## Summary

Moves WASM logging initialization to a manual public API to prevent debug logs (e.g., from `globset`) from leaking to stdout by default. ~~Following maintainer feedback, `console_log` is now an optional dependency and the logger is only initialized if the user explicitly calls `init_logging(level)`.~~

Fixes #22535

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:44_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Renamed from "fix(wasm): move logging to manual initialization via init_logging" to "wasm: Require explicit logging initialization" by @MichaReiser on 2026-01-15 08:31_

---

_Review requested from @carljm by @MichaReiser on 2026-01-15 08:31_

---

_Review requested from @MichaReiser by @MichaReiser on 2026-01-15 08:31_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-15 08:31_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-15 08:31_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-15 08:31_

---

_Label `playground` added by @MichaReiser on 2026-01-15 08:34_

---
