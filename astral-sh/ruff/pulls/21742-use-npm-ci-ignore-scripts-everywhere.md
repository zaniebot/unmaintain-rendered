```yaml
number: 21742
title: "Use `npm ci --ignore-scripts` everywhere"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/npm-no-scripts
created_at: 2025-12-01T21:48:34Z
updated_at: 2025-12-01T22:13:55Z
url: https://github.com/astral-sh/ruff/pull/21742
synced_at: 2026-01-12T15:57:32Z
```

# Use `npm ci --ignore-scripts` everywhere

---

_@woodruffw_

## Summary

This is more reproducible, and avoids unnecessary build-time script execution.

## Test Plan

See what happens in CI. I've confirmed that the SchemaStore change in particular works by making the same change to uv: https://github.com/astral-sh/uv/pull/16915

---

_Assigned to @woodruffw by @woodruffw on 2025-12-01 21:48_

---

_Label `internal` added by @woodruffw on 2025-12-01 21:48_

---

_Review requested from @carljm by @woodruffw on 2025-12-01 21:48_

---

_Review requested from @MichaReiser by @woodruffw on 2025-12-01 21:48_

---

_Review requested from @AlexWaygood by @woodruffw on 2025-12-01 21:48_

---

_Review requested from @sharkdp by @woodruffw on 2025-12-01 21:48_

---

_Review requested from @dcreager by @woodruffw on 2025-12-01 21:48_

---

_@charliermarsh approved on 2025-12-01 21:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 22:06_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-01 22:08_

---

_Merged by @woodruffw on 2025-12-01 22:13_

---

_Closed by @woodruffw on 2025-12-01 22:13_

---

_Branch deleted on 2025-12-01 22:13_

---
