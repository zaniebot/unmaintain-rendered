```yaml
number: 699
title: conflicting-declarations error can contain duplicate types
type: issue
state: closed
author: lipefree
labels:
  - diagnostics
assignees: []
created_at: 2025-06-25T09:03:26Z
updated_at: 2025-06-26T10:24:42Z
url: https://github.com/astral-sh/ty/issues/699
synced_at: 2026-01-10T02:07:36Z
```

# conflicting-declarations error can contain duplicate types

---

_Issue opened by @lipefree on 2025-06-25 09:03_

### Summary

For the following snippet : 

```py
def _(flag1: bool, flag2: bool):
    if flag1:
        x: str
    elif flag2:
        x: int
    else:
        x: int

    x = 1
```

We will have the following error : ``error[conflicting-declarations]: Conflicting declared types for `x`: str, int, int``. However, the list of conflicting declared types should consist of unique elements. 

[playground reproduction](https://play.ty.dev/bed84d43-1679-45b6-9a4a-06c94354c6e3)

I think it caused by the implementation logic, more specifically when [adding types to the list of conflicting declared types](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/place.rs#L957-L959), the code only checks if the new type is different from the first one but it should do it for all elements already in the list. 

If it is caused by that, I think I can handle this issue!

### Version

ty 0.0.1-alpha.11

---

_Label `diagnostics` added by @sharkdp on 2025-06-25 09:08_

---

_Comment by @sharkdp on 2025-06-25 09:10_

Thank you for reporting this.

I am working on a PR that touches that area anyway, I think I can include a simple fix there by using an `OrderSet` instead of a `Vec`.

---

_Closed by @sharkdp on 2025-06-26 10:24_

---
