```yaml
number: 11796
title: "[red-knot] switch from PreorderVisitor to regular Visitor"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-07T15:15:43Z
updated_at: 2024-07-19T20:26:04Z
url: https://github.com/astral-sh/ruff/issues/11796
synced_at: 2026-01-10T11:09:53Z
```

# [red-knot] switch from PreorderVisitor to regular Visitor

---

_Issue opened by @carljm on 2024-06-07 15:15_

We don't need the extra features provided by `PreorderVisitor` (`enter_node`, `TraversalSignal` etc), and we prefer evaluation-order visit (which the regular visitor does) over source-order visit (which `PreorderVisitor` does).

Also in this issue we could consider renaming the visitors themselves; they are both preorder visitors (to the extent that description is even meaningful for this style of AST visitor; if you can override `visit_*` you can do things both before and after visiting children, so you can implement both preorder and postorder traversal) so it doesn't make sense to distinguish them on that axis. We should probably rename `PreorderVisitor` to `SourceOrderVisitor`.

---

_Label `red-knot` added by @carljm on 2024-06-07 15:15_

---

_Comment by @carljm on 2024-06-07 16:33_

#11798 took care of the rename!

---

_Comment by @MichaReiser on 2024-07-17 06:54_

We must have done this in some refactor. @carljm I let you double check.

---

_Closed by @carljm on 2024-07-19 20:26_

---
