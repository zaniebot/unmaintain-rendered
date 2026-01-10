```yaml
number: 17229
title: "[red-knot] Remove global `dead_code` from red-knot-server"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/knot-server-dead-code
created_at: 2025-04-06T08:32:56Z
updated_at: 2025-04-06T21:09:26Z
url: https://github.com/astral-sh/ruff/pull/17229
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Remove global `dead_code` from red-knot-server

---

_Pull request opened by @MichaReiser on 2025-04-06 08:32_

## Summary

Use more local `expect(dead_code)` suppressions instead of a global `allow(dead_code)` in `lib.rs`. 

Remove some methods that are either easy to add later, are less likely to be needed for red knot, or it's unclear if we'd add it the same way as in ruff.



---

_Review requested from @carljm by @MichaReiser on 2025-04-06 08:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-06 08:32_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-06 08:32_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-06 08:32_

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-06 08:33_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-06 08:33_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-06 08:33_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-06 08:33_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-06 08:33_

---

_Label `red-knot` added by @MichaReiser on 2025-04-06 08:33_

---

_Comment by @github-actions[bot] on 2025-04-06 08:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-06 08:54_

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

_@carljm approved on 2025-04-06 19:49_

---

_Merged by @MichaReiser on 2025-04-06 21:09_

---

_Closed by @MichaReiser on 2025-04-06 21:09_

---

_Branch deleted on 2025-04-06 21:09_

---
