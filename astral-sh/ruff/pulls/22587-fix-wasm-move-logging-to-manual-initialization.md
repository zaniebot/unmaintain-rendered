```yaml
number: 22587
title: "fix(wasm): move logging to manual initialization via init_logging"
type: pull_request
state: open
author: Jkhall81
labels: []
assignees: []
base: main
head: fix/wasm-logging-init-22535
created_at: 2026-01-14T22:33:02Z
updated_at: 2026-01-14T22:44:46Z
url: https://github.com/astral-sh/ruff/pull/22587
synced_at: 2026-01-14T23:42:30Z
```

# fix(wasm): move logging to manual initialization via init_logging

---

_@Jkhall81_

## Summary

Moves WASM logging initialization to a manual public API to prevent debug logs (e.g., from `globset`) from leaking to stdout by default. Following maintainer feedback, `console_log` is now an optional dependency and the logger is only initialized if the user explicitly calls `init_logging(level)`.

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
