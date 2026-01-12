```yaml
number: 10574
title: "`Q000` autofix failing in forward reference type annotations"
type: issue
state: closed
author: augustelalande
labels:
  - bug
assignees: []
created_at: 2024-03-25T17:08:37Z
updated_at: 2024-03-25T17:10:18Z
url: https://github.com/astral-sh/ruff/issues/10574
synced_at: 2026-01-12T15:54:50Z
```

# `Q000` autofix failing in forward reference type annotations

---

_@augustelalande_

The `Q000` autofix is generating syntax errors in code with forward reference type annotations which include string literals.
```python
def foo(x: "Literal['a', 'b', 'c']") -> None:
    ...
```
autofixed to 
```python
def foo(x: "Literal["a", "b", "c"]") -> None:
    ...
```

this was found by https://github.com/qarmin/Automated-Fuzzer/actions/runs/8412442975

---

_Comment by @AlexWaygood on 2024-03-25 17:10_

Thanks! I think the underlying issue is the same as #10546, so I'll close this in favour of that one 👍

---

_Closed by @AlexWaygood on 2024-03-25 17:10_

---

_Label `bug` added by @MichaReiser on 2024-03-25 17:10_

---
