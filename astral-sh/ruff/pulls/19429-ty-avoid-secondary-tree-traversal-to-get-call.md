```yaml
number: 19429
title: "[ty] Avoid secondary tree traversal to get call expression for keyword arguments"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/call-expr-goto-target
created_at: 2025-07-19T13:35:26Z
updated_at: 2025-07-19T16:21:29Z
url: https://github.com/astral-sh/ruff/pull/19429
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Avoid secondary tree traversal to get call expression for keyword arguments

---

_Pull request opened by @MichaReiser on 2025-07-19 13:35_

## Summary


Follow up to https://github.com/astral-sh/ruff/pull/19417/files#r2215520292

Extract the enclosing `CallExpr` when resolving the `KeywordArgument` goto target (where we have access to the ancestor chain) 
instead of doing another tree traversal. 

Not only is this faster. It's also less confusing (I assumed that there was more to it then just picking the parent call expression because it did use a second traversal).

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-07-19 13:35_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-19 13:35_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-19 13:35_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-19 13:35_

---

_Label `internal` added by @MichaReiser on 2025-07-19 13:35_

---

_Label `ty` added by @MichaReiser on 2025-07-19 13:35_

---

_Review requested from @UnboundVariable by @MichaReiser on 2025-07-19 13:35_

---

_Comment by @github-actions[bot] on 2025-07-19 13:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@UnboundVariable approved on 2025-07-19 15:29_

Thanks, I misunderstood the feedback from your previous code review. This looks fine to me. 

---

_Comment by @MichaReiser on 2025-07-19 16:21_

> Thanks, I misunderstood the feedback from your previous code review. This looks fine to me.

I'm sorry that my comment was unclear. 

---

_Merged by @MichaReiser on 2025-07-19 16:21_

---

_Closed by @MichaReiser on 2025-07-19 16:21_

---

_Branch deleted on 2025-07-19 16:21_

---
