---
number: 15224
title: Inconsistent behavior in D405 with capitalization in Google docstrings
type: issue
state: closed
author: mgilson
labels:
  - bug
  - docstring
assignees: []
created_at: 2025-01-02T16:53:46Z
updated_at: 2025-01-07T03:13:37Z
url: https://github.com/astral-sh/ruff/issues/15224
synced_at: 2026-01-07T13:12:16-06:00
---

# Inconsistent behavior in D405 with capitalization in Google docstrings

---

_Issue opened by @mgilson on 2025-01-02 16:53_

Consider the following simple snippet:

```py
def func(a: int) -> int:
    """A docstring.

    args:
        a: an integer.

    returns:
        An integer.
    """
    return a
```

This triggers a D405 warning on line 4 -- it's unhappy that `args:` isn't `Args:`.  However, it does NOT trigger a similar warning for the `returns:` section.  If we get rid of the Args section then a D405 error is reported.

```py
def func(a: int) -> int:
    """A docstring.

    returns:
        An integer.
    """
    return a
```

I suspect that this is a bug (I wouldn't expect the presence of one section to have any impact on whether that section should be capitalized).

---

_Label `bug` added by @dylwil3 on 2025-01-02 18:11_

---

_Label `docstring` added by @AlexWaygood on 2025-01-03 13:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-07 03:03_

---

_Referenced in [astral-sh/ruff#15311](../../astral-sh/ruff/pulls/15311.md) on 2025-01-07 03:04_

---

_Closed by @charliermarsh on 2025-01-07 03:13_

---

_Closed by @charliermarsh on 2025-01-07 03:13_

---
