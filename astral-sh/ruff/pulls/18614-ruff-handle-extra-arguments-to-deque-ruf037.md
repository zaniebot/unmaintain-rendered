```yaml
number: 18614
title: "[`ruff`] Handle extra arguments to `deque` (`RUF037`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/fix-ruf037
created_at: 2025-06-10T19:15:25Z
updated_at: 2025-06-12T13:07:19Z
url: https://github.com/astral-sh/ruff/pull/18614
synced_at: 2026-01-10T18:45:04Z
```

# [`ruff`] Handle extra arguments to `deque` (`RUF037`)

---

_Pull request opened by @ntBre on 2025-06-10 19:15_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/18612 by:
- Bailing out without a fix in the case of `*args`, which I don't think we can fix reliably
- Using an `Edit::deletion` from `remove_argument` instead of an `Edit::range_replacement` in the presence of unrecognized keyword arguments

I thought we could always switch to the `Edit::deletion` approach initially, but it caused problems when `maxlen` was passed positionally, which we didn't have any existing tests for.

The replacement fix can easily delete comments, so I also marked the fix unsafe in these cases and updated the docs accordingly.

## Test Plan

New test cases derived from the issue.

## Stabilization

These are pretty significant changes, much like those to PYI059 in https://github.com/astral-sh/ruff/pull/18611 (and based a bit on the implementation there!), so I think it probably makes sense to un-stabilize this for the 0.12 release, but I'm open to other thoughts there.


---

_Label `bug` added by @ntBre on 2025-06-10 19:15_

---

_Label `fixes` added by @ntBre on 2025-06-10 19:15_

---

_Comment by @github-actions[bot] on 2025-06-10 19:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-06-10 19:52_

---

_@MichaReiser reviewed on 2025-06-11 05:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF037_RUF037.py.snap`:174 on 2025-06-11 05:55_

Very nit picky, but I think this is unsafe if the dict contains an `iterable` keyword. The same if there are any other keywords around:

```
>>> from collections import deque
>>> a = deque(iterable=[1, 2])
>>> list(a)
[1, 2]
>>> list(a)
KeyboardInterrupt
>>> a = deque([1], iterable=[1, 2])
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    a = deque([1], iterable=[1, 2])
TypeError: argument for deque() given by name ('iterable') and position (1)
```

We should mark the fix as unsafe if there are any other arguments.


---

_@MichaReiser approved on 2025-06-11 05:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF037_RUF037.py.snap`:174 on 2025-06-11 11:02_

Wow nice catch! I added this case:

```py
def f():
    deque([], iterable=[])
```

and realized that I was mishandling the range passed to `remove_argument` because this was returning an `Err`! I was only passing the `ArgOrKeyword::value` not the whole `ArgOrKeyword`.

I fixed that, and then also marked the fix unsafe if we find an unrecognized number of arguments.

---

_@ntBre reviewed on 2025-06-11 11:02_

---

_Merged by @ntBre on 2025-06-12 13:07_

---

_Closed by @ntBre on 2025-06-12 13:07_

---

_Branch deleted on 2025-06-12 13:07_

---
