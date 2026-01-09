---
number: 15307
title: "`@no_type_check` is not respected for string annotations"
type: issue
state: open
author: InSyncWithFoo
labels:
  - bug
assignees: []
created_at: 2025-01-06T19:45:40Z
updated_at: 2025-02-19T05:44:00Z
url: https://github.com/astral-sh/ruff/issues/15307
synced_at: 2026-01-07T13:12:16-06:00
---

# `@no_type_check` is not respected for string annotations

---

_Issue opened by @InSyncWithFoo on 2025-01-06 19:45_

For some reason, the logic implemented in #15215 failed to suppress this ([playground](https://play.ruff.rs/cef33e0d-088e-4c49-9ead-ca5d07792f7f)):

```python
import typing

@typing.no_type_check
def f(arg: "'A' or ''") -> None: ...
#            ^ F821
```

According to `println!()`, when checking the nested expression, the semantic model is in neither `NO_TYPE_CHECK` nor `STRING_TYPE_DEFINITION` context.

---

_Renamed from "`@no_type_check` is not respected when the stringified type expression is complex" to "`@no_type_check` is not respected" by @InSyncWithFoo on 2025-01-06 19:47_

---

_Label `bug` added by @MichaReiser on 2025-01-06 19:47_

---

_Renamed from "`@no_type_check` is not respected" to "`@no_type_check` is not respected for string annotations" by @dhruvmanila on 2025-01-08 04:54_

---

_Comment by @kuraga on 2025-02-16 19:56_

The same issue.

---
