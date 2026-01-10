```yaml
number: 19567
title: "[ty] Implemented support for \"selection range\" language server feature"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: selection_range
created_at: 2025-07-25T23:29:51Z
updated_at: 2025-07-26T16:08:40Z
url: https://github.com/astral-sh/ruff/pull/19567
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Implemented support for "selection range" language server feature

---

_Pull request opened by @UnboundVariable on 2025-07-25 23:29_

This PR adds support for the "selection range" language server feature. This feature was recently requested by a ty user in [this feature request](https://github.com/astral-sh/ty/issues/882).

This feature allows a client to implement "smart selection expansion" based on the structure of the parse tree. For example, if you type "shift-ctrl-right-arrow" in VS Code, the current selection will be expanded to include the parent AST node. Conversely, "shift-ctrl-left-arrow" shrinks the selection.

We will probably need to tune the granularity of selection expansion based on user feedback. The initial implementation includes most AST nodes, but users may find this to be too fine-grained. We have the option of skipping some AST nodes that are not as meaningful when editing code.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-25 23:29_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-25 23:29_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-25 23:29_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-25 23:29_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-25 23:29_

---

_Comment by @github-actions[bot] on 2025-07-25 23:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-26 10:35_

---

_Label `ty` added by @MichaReiser on 2025-07-26 10:35_

---

_@MichaReiser approved on 2025-07-26 10:36_

Nice

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-26 11:40_

---

_Merged by @UnboundVariable on 2025-07-26 16:08_

---

_Closed by @UnboundVariable on 2025-07-26 16:08_

---

_Branch deleted on 2025-07-26 16:08_

---
