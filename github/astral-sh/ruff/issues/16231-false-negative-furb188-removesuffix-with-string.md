---
number: 16231
title: False negative - FURB188 removesuffix with string literal
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2025-02-18T15:11:42Z
updated_at: 2025-02-18T18:52:27Z
url: https://github.com/astral-sh/ruff/issues/16231
synced_at: 2026-01-07T13:12:16-06:00
---

# False negative - FURB188 removesuffix with string literal

---

_Issue opened by @Skylion007 on 2025-02-18 15:11_

### Description

```python
a = "sjdfaskldjfakljklfoo"
if a.endswith("foo"):
    a = a[: -len("foo")]
print(a)
```
should be transformed to
```python
a = "sjdfaskldjfakljklfoo"
a = a.removesuffx("foo")
print(a)
```
when FURB188 is enabled.  Other similar trivial cases are caught as seen by the changes I made in PyTorch for https://github.com/pytorch/pytorch/pull/146997 so I assume it's an error in the rule logic where some corner case is not properly handled.

Ruff version is `ruff 0.9.6`
Command is `ruff check --select=FURB188 --target-version py39 test_FURB188.py`

Interestingly:
```python
a = "sjdfaskldjfakljklfoo"
suffix = "foo"
if a.endswith(suffix):
    a = a[: -len(suffix)]
print(a)
```
is properly flagged currently. Therefore, ruff only this issue with string literals, and only for suffixes accessed in this pattern interestingly enough.

---

_Label `bug` added by @ntBre on 2025-02-18 15:22_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-18 18:03_

---

_Referenced in [astral-sh/ruff#16237](../../astral-sh/ruff/pulls/16237.md) on 2025-02-18 18:29_

---

_Closed by @dylwil3 on 2025-02-18 18:52_

---
