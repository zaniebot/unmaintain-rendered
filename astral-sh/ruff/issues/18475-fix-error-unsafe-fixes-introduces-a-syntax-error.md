```yaml
number: 18475
title: "[Fix error] unsafe-fixes introduces a syntax error"
type: issue
state: closed
author: zegervdv
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-05T08:40:16Z
updated_at: 2025-06-13T06:44:17Z
url: https://github.com/astral-sh/ruff/issues/18475
synced_at: 2026-01-12T15:54:56Z
```

# [Fix error] unsafe-fixes introduces a syntax error

---

_@zegervdv_

When running `ruff check --fix --unsafe-fixes` (no extra settings or config) on the following code snippet:

``` python
foo_tooltip = (
    lambda x, data: f"\nfoo: {data['foo'][int(x)]}"
    if data["foo"] is not None
    else ""
)

```

Ruff introduces a syntax error and reverts the fix.
The rule code to fix is E731: 
```
test.py:1:1: E731 Do not assign a `lambda` expression, use a `def`
```

---

_Label `bug` added by @MichaReiser on 2025-06-05 10:14_

---

_Label `fixes` added by @MichaReiser on 2025-06-05 10:14_

---

_Comment by @ntBre on 2025-06-05 12:36_

Thanks, I can reproduce this by applying the fix in the [playground](https://play.ruff.rs/c078ff2b-c335-47c3-9e60-8b22d971cf00). The output looks like this:

```python
def foo_tooltip(x, data):
    return f"\nfoo: {data['foo'][int(x)]}"
    if data["foo"] is not None
    else ""
```

It looks like wrapping the returned value in parentheses fixes it.

---

_Closed by @MichaReiser on 2025-06-13 06:44_

---
