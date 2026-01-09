---
number: 17347
title: "[`pylint`] Extend `PLW0133` to also detect user-defined exceptions"
type: issue
state: closed
author: LoicRiegel
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-04-11T08:13:07Z
updated_at: 2025-12-09T18:49:56Z
url: https://github.com/astral-sh/ruff/issues/17347
synced_at: 2026-01-07T13:12:16-06:00
---

# [`pylint`] Extend `PLW0133` to also detect user-defined exceptions

---

_Issue opened by @LoicRiegel on 2025-04-11 08:13_

### Summary

From the [rule's documentation](https://docs.astral.sh/ruff/rules/useless-exception-statement/#useless-exception-statement-plw0133) (PLW0133):

> Known problems
>
> This rule only detects built-in exceptions, like ValueError, and does not catch user-defined exceptions.


So currently this is caught:
```py
def foo():
    ValueError("...")
```
But not this. But I would love if it would, it would have spared me 1hr of debugging this morning :)
```py
class MyCustomError(Exception):
    ...

def foo():
    MyCustomError("...")
```

---

_Label `rule` added by @MichaReiser on 2025-04-11 08:20_

---

_Label `type-inference` added by @MichaReiser on 2025-04-11 08:20_

---

_Referenced in [astral-sh/ruff#21382](../../astral-sh/ruff/pulls/21382.md) on 2025-11-11 15:23_

---

_Renamed from "Extend PLW0133 to also detect user-defined exceptions" to "[`pylint`] Extend `PLW0133` to also detect user-defined exceptions" by @amyreese on 2025-11-17 22:17_

---

_Closed by @ntBre on 2025-12-09 18:49_

---
