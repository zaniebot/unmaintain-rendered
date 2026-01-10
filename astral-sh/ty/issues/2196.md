```yaml
number: 2196
title: "explicit annotation for `defaultdict(float)` is not accepted"
type: issue
state: closed
author: dimbleby
labels: []
assignees: []
created_at: 2025-12-23T23:36:22Z
updated_at: 2025-12-24T00:12:02Z
url: https://github.com/astral-sh/ty/issues/2196
synced_at: 2026-01-10T01:56:41Z
```

# explicit annotation for `defaultdict(float)` is not accepted

---

_Issue opened by @dimbleby on 2025-12-23 23:36_

### Summary

Some sort of `float | int` vs `float` confusion - I see the related FAQ - I did not find any duplicate issues but perhaps there is one out there

```python
from collections import defaultdict

foo: dict[str, float] = defaultdict(float)
```
with result
```
error[invalid-assignment]: Object of type `defaultdict[Unknown, float]` is not assignable to `dict[str, int | float]`
 --> foo.py:3:6
  |
1 | from collections import defaultdict
2 |
3 | foo: dict[str, float] = defaultdict(float)
  |      ----------------   ^^^^^^^^^^^^^^^^^^ Incompatible value of type `defaultdict[Unknown, float]`
  |      |
  |      Declared type
  |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```

awkward if I am also trying to annotate my code in a way that will satisfy mypy, which often insists that it needs a type annotation in such cases

### Version

0.0.6

---

_Comment by @carljm on 2025-12-23 23:57_

Thanks for the report! Yeah, we'll either need to relax the rules around invariance for `float` vs `float | int`, or make sure we always have type context available in such cases.

I think this alternative might make both mypy and ty happy for now?

```py
foo = defaultdict[str, float](float)

---

_Added to milestone `Stable` by @carljm on 2025-12-23 23:57_

---

_Comment by @AlexWaygood on 2025-12-24 00:01_

Does this still repro on the branch for https://github.com/astral-sh/ruff/pull/21930?

---

_Comment by @carljm on 2025-12-24 00:11_

> Does this still repro on the branch for [astral-sh/ruff#21930](https://github.com/astral-sh/ruff/pull/21930)?

Good call! The last few issues I tried, that PR didn't fix, but it makes sense that it would fix this one, and it does!

---

_Comment by @carljm on 2025-12-24 00:12_

Closing as dupe of #1576, since the PR to close that issue fixes this.

---

_Closed by @carljm on 2025-12-24 00:12_

---
