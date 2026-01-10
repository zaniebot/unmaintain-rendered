```yaml
number: 19341
title: "[ty] distinguish references from definitions in `infer_nonlocal`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
assignees: []
merged: true
base: main
head: jack/nonlocal_skip_use_only
created_at: 2025-07-14T21:47:57Z
updated_at: 2025-07-15T14:55:42Z
url: https://github.com/astral-sh/ruff/pull/19341
synced_at: 2026-01-10T17:58:13Z
```

# [ty] distinguish references from definitions in `infer_nonlocal`

---

_Pull request opened by @oconnor663 on 2025-07-14 21:47_

The initial implementation of `infer_nonlocal` landed in https://github.com/astral-sh/ruff/pull/19112 fails to report an error for this example:

```py
x = 1
def f():
    # This is only a usage of `x`, not a definition. It shouldn't be
    # enough to make the `nonlocal` statement below allowed.
    print(x)
    def g():
        nonlocal x
```

Fix this by continuing to walk enclosing scopes when the place we've found isn't bound, declared, or `nonlocal`.

---

_Review requested from @carljm by @oconnor663 on 2025-07-14 21:47_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-14 21:47_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-14 21:47_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-14 21:47_

---

_Comment by @github-actions[bot] on 2025-07-14 21:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "distinguish references from definitions in `infer_nonlocal`" to "[ty] distinguish references from definitions in `infer_nonlocal`" by @AlexWaygood on 2025-07-14 22:11_

---

_Label `ty` added by @AlexWaygood on 2025-07-14 22:11_

---

_@AlexWaygood approved on 2025-07-15 10:39_

---

_Merged by @oconnor663 on 2025-07-15 14:55_

---

_Closed by @oconnor663 on 2025-07-15 14:55_

---

_Branch deleted on 2025-07-15 14:55_

---
