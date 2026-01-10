---
number: 7720
title: "E731: Preserve multiline string when fixing."
type: issue
state: closed
author: DerBiasto
labels:
  - fixes
assignees: []
created_at: 2023-09-30T03:48:58Z
updated_at: 2024-12-23T09:29:47Z
url: https://github.com/astral-sh/ruff/issues/7720
synced_at: 2026-01-10T01:22:47Z
---

# E731: Preserve multiline string when fixing.

---

_Issue opened by @DerBiasto on 2023-09-30 03:48_

If I have this statement assigning a lambda, which returns a multiline string, to a variable:

``` python3
x = lambda: """
        a
    b
"""
```

The automatic fix turns it into

``` python3
def x():
    return "\n        a\n    b\n"
```

This technically preserves the whitespace correctly, but destroys readability.

I don't know if it is possible, but it would be great, if the fix could preserve the original multiline string:

``` python3
def x():
    return """
        a
    b
"""
```

---

_Label `autofix` added by @charliermarsh on 2023-09-30 09:58_

---

_Referenced in [astral-sh/ruff#7799](../../astral-sh/ruff/issues/7799.md) on 2023-10-04 04:54_

---

_Referenced in [astral-sh/ruff#15097](../../astral-sh/ruff/pulls/15097.md) on 2024-12-22 02:05_

---

_Closed by @MichaReiser on 2024-12-23 09:29_

---
