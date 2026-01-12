```yaml
number: 17182
title: Enable overindented docs lint
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/enable-overindented
created_at: 2025-04-03T19:11:30Z
updated_at: 2025-04-03T19:27:13Z
url: https://github.com/astral-sh/ruff/pull/17182
synced_at: 2026-01-12T15:56:00Z
```

# Enable overindented docs lint

---

_@MichaReiser_

## Summary

It turns out that `a.` isn't a list format supported by rustdoc. I changed the documentation to use `1.`, `2.` instead.

## Test Plan

`cargo clippy`


---

_Review requested from @carljm by @MichaReiser on 2025-04-03 19:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-03 19:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-03 19:11_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-03 19:11_

---

_Label `internal` added by @MichaReiser on 2025-04-03 19:11_

---

_Comment by @github-actions[bot] on 2025-04-03 19:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-03 19:20_

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

_Merged by @MichaReiser on 2025-04-03 19:27_

---

_Closed by @MichaReiser on 2025-04-03 19:27_

---

_Branch deleted on 2025-04-03 19:27_

---
