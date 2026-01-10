```yaml
number: 19366
title: "[ty] Allow `-qq` for silent output mode"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: zb/silent
created_at: 2025-07-15T17:00:24Z
updated_at: 2025-07-15T17:08:22Z
url: https://github.com/astral-sh/ruff/pull/19366
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Allow `-qq` for silent output mode

---

_Pull request opened by @zanieb on 2025-07-15 17:00_

This matches uv's behavior.

Briefly discussed at https://github.com/astral-sh/ruff/pull/19233#discussion_r2197930360

I think the most useful case is to avoid piping to `/dev/null` which hard to do properly in a cross-platform script.

---

_Label `cli` added by @zanieb on 2025-07-15 17:00_

---

_Label `ty` added by @zanieb on 2025-07-15 17:00_

---

_Review requested from @carljm by @zanieb on 2025-07-15 17:00_

---

_Review requested from @MichaReiser by @zanieb on 2025-07-15 17:00_

---

_Review requested from @AlexWaygood by @zanieb on 2025-07-15 17:00_

---

_Review requested from @sharkdp by @zanieb on 2025-07-15 17:00_

---

_Review requested from @dcreager by @zanieb on 2025-07-15 17:00_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-15 17:01_

---

_@MichaReiser approved on 2025-07-15 17:01_

---

_Comment by @github-actions[bot] on 2025-07-15 17:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @zanieb on 2025-07-15 17:08_

---

_Closed by @zanieb on 2025-07-15 17:08_

---

_Branch deleted on 2025-07-15 17:08_

---
