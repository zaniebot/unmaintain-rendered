---
number: 16964
title: document side effect of FURB192
type: issue
state: open
author: spaceone
labels:
  - bug
  - documentation
  - preview
assignees: []
created_at: 2025-03-25T06:26:46Z
updated_at: 2025-09-03T18:42:13Z
url: https://github.com/astral-sh/ruff/issues/16964
synced_at: 2026-01-10T01:22:58Z
---

# document side effect of FURB192

---

_Issue opened by @spaceone on 2025-03-25 06:26_

### Summary

The `FURB192` fixer introduces a side effect for empty sequences: a different exception is raised, which could at least be documented in the rule.
 
```python
>>> nums = []
>>> max(nums)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: max() arg is an empty sequence
>>> sorted(nums)[-1]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```


### Version

0.11.2

---

_Label `documentation` added by @ntBre on 2025-03-25 18:01_

---

_Comment by @ntBre on 2025-09-03 18:42_

I was checking FURB192 for stabilization in 0.13, and I don't think this is just a documentation issue, despite my earlier label. I believe this kind of behavior change should actually make the fix unsafe. We'd have to prove that the sequence is non-empty to call the fix safe conclusively, so this probably means it always needs to be unsafe, not just when the `reverse` keyword is provided, as is currently the case.

---

_Label `bug` added by @ntBre on 2025-09-03 18:42_

---

_Label `preview` added by @ntBre on 2025-09-03 18:42_

---
