---
number: 14798
title: RUF052 fix is unsafe when renaming TypeVars
type: issue
state: closed
author: oguzhanmeteozturk
labels:
  - bug
  - rule
  - fixes
  - preview
assignees: []
created_at: 2024-12-05T22:57:11Z
updated_at: 2024-12-06T17:05:51Z
url: https://github.com/astral-sh/ruff/issues/14798
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF052 fix is unsafe when renaming TypeVars

---

_Issue opened by @oguzhanmeteozturk on 2024-12-05 22:57_

The automatic fix for RUF052 is unsafe when applied to TypeVar definitions. It renames ``` _T = TypeVar("_T")``` to``` T = TypeVar("_T")``` resulting in invalid code. The TypeVar name string must match the assigned variable name to maintain correctness, and the resulting T = TypeVar("_T") violates this requirement.

The underscore is often used to indicate private or internal TypeVars and is a common and valid pattern in python.

---

_Label `bug` added by @dylwil3 on 2024-12-05 23:05_

---

_Label `rule` added by @dylwil3 on 2024-12-05 23:05_

---

_Label `fixes` added by @dylwil3 on 2024-12-05 23:05_

---

_Label `preview` added by @dylwil3 on 2024-12-05 23:06_

---

_Comment by @AlexWaygood on 2024-12-05 23:28_

Thanks for opening the issue! Interesting, are you defining TypeVars inside functions? I thought RUF052 only applied inside function scopes

---

_Comment by @oguzhanmeteozturk on 2024-12-05 23:34_

Tests define TypeVars internally sometimes

---

_Comment by @AlexWaygood on 2024-12-05 23:37_

Just to make sure I understand -- something like this?

```py
def test_foo():
    _T = TypeVar("_T")
    
    def bar(x: _T) -> _T:
        ...  # some logic here
```

---

_Comment by @oguzhanmeteozturk on 2024-12-06 00:55_

Yes, very similar. Not sure how relevant it is, but the exact point where it broke the test involves _T being passed as an argument to a FUT within a context (pytest.raises in this case).

---

_Comment by @AlexWaygood on 2024-12-06 00:59_

Great, thanks. I'd argue that this is within the purview of RUF052 -- since the TypeVar is a local variable, it's private to the function even without the underscore; leading underscores are more usually used to indicate unused variables in function scopes, I think.

However, we obviously shouldn't be breaking TypeVar definitions by changing the name the TypeVar is assigned to without changing the string passed into the constructor. We either need to make our renaming logic more sophisticated and/or mark the autofix as unsafe in more cases.

Thanks for the report!

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-12-06 13:07_

---

_Referenced in [astral-sh/ruff#14819](../../astral-sh/ruff/pulls/14819.md) on 2024-12-06 14:21_

---

_Closed by @AlexWaygood on 2024-12-06 17:05_

---
