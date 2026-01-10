```yaml
number: 14731
title: "[red-knot] Incorrect subtyping behavior of `Literal` instances and `_SpecialForm`"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2024-12-02T14:14:01Z
updated_at: 2024-12-03T11:03:27Z
url: https://github.com/astral-sh/ruff/issues/14731
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Incorrect subtyping behavior of `Literal` instances and `_SpecialForm`

---

_Issue opened by @sharkdp on 2024-12-02 14:14_

We have a correct test for the subtype relationship between these two types here:

https://github.com/astral-sh/ruff/blob/6dfe125f44352f5fdff19f6d47fc33ac7a6e86ad/crates/red_knot_python_semantic/src/types.rs#L3208C9-L3208C72

But the opposite is also true, at the moment. We should fix this and add a `is_not_subtype_of` test case.

---

_Label `bug` added by @sharkdp on 2024-12-02 14:14_

---

_Label `red-knot` added by @sharkdp on 2024-12-02 14:14_

---

_Closed by @sharkdp on 2024-12-03 11:03_

---

_Closed by @sharkdp on 2024-12-03 11:03_

---
