```yaml
number: 14996
title: "`used-dummy-variable` (`RUF052`) - false positive on private global variable when it has a usage in two functions"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-16T00:14:42Z
updated_at: 2025-01-14T07:36:41Z
url: https://github.com/astral-sh/ruff/issues/14996
synced_at: 2026-01-12T15:54:54Z
```

# `used-dummy-variable` (`RUF052`) - false positive on private global variable when it has a usage in two functions

---

_@DetachHead_

```py
_bar: int


def foo():
    global _bar # error: Local dummy variable `_bar` is accessed
    _bar = 1


def baz():
    print(_bar)
```

https://play.ruff.rs/7b9e5645-c159-4e00-8b84-49183a6c31c4

---

_Label `bug` added by @MichaReiser on 2024-12-16 07:32_

---

_Label `help wanted` added by @MichaReiser on 2024-12-16 07:32_

---

_Comment by @MichaReiser on 2024-12-16 07:32_

Thanks

---

_Closed by @MichaReiser on 2025-01-14 07:36_

---
