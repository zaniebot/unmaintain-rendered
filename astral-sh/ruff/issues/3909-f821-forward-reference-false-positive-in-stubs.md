```yaml
number: 3909
title: "`F821` forward reference false positive in stubs (`.pyi` files)"
type: issue
state: closed
author: Avasam
labels:
  - bug
assignees: []
created_at: 2023-04-07T23:21:43Z
updated_at: 2023-04-07T23:40:35Z
url: https://github.com/astral-sh/ruff/issues/3909
synced_at: 2026-01-12T15:54:44Z
```

# `F821` forward reference false positive in stubs (`.pyi` files)

---

_@Avasam_

Given this code:
```py
class A(B): ...
class B: ...
```

Does raise `F821 Undefined name` in python files, but should not raise in stubs `.pyi` files.

Whilst it is true that it's a best practice to avoid forward references, sometimes it can't be avoided in more complex cases. "Undefined name" is also simply not true in this context.

---

_Comment by @charliermarsh on 2023-04-07 23:36_

Possibly the same as #3011.

---

_Label `bug` added by @charliermarsh on 2023-04-07 23:36_

---

_Comment by @charliermarsh on 2023-04-07 23:36_

(I think we need to implement different visitation rules for `.pyi` files.)

---

_Comment by @Avasam on 2023-04-07 23:39_

> Possibly the same as #3011.

It does look like I duplicated that issue. Sorry I do try to search first, I must've mistyped the rule code as no results came up. Feel free to close this in favor of #3011

---

_Comment by @charliermarsh on 2023-04-07 23:40_

No worries, sadly I have encyclopedic knowledge of our issues so it's easy for me to recall duplicates :joy:

---

_Closed by @charliermarsh on 2023-04-07 23:40_

---
