---
number: 13581
title: "B905's autofix sets strict=False, but the suggested fix is strict=True"
type: issue
state: closed
author: mdbernard
labels:
  - rule
assignees: []
created_at: 2024-10-01T04:54:19Z
updated_at: 2024-10-07T11:56:18Z
url: https://github.com/astral-sh/ruff/issues/13581
synced_at: 2026-01-07T13:12:15-06:00
---

# B905's autofix sets strict=False, but the suggested fix is strict=True

---

_Issue opened by @mdbernard on 2024-10-01 04:54_

The [docs for B905](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/#zip-without-explicit-strict-b905) state the suggested fix is to use `strict=True`, but the autofix introduced [here](https://github.com/astral-sh/ruff/pull/9176) autofixes with `strict=False`.

Either the docs or the autofix should be updated to match the other.

I would argue that the autofix should be changed to `strict=True`, since that is the option less likely to cause unexpected runtime behavior for users. It might cause runtime exceptions to be raised, but I personally would prefer those to silently continuing with potentially unintended behavior.

----

Keywords: B905, zip-without-explicit-strict

Current ruff version: 0.6.6

Minimal snippet:
```python
zip(a, b)
```

Command invoked:
`ruff check example.py --target-version=py311 --select=B905`

Output:
```
example.py:1:1: B905 [*] `zip()` without an explicit `strict=` parameter
  |
1 | zip(a, b)
  | ^^^^^^^^^ B905
  |
  = help: Add explicit `strict=False`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Expected output:
```
...
  = help: Add explicit `strict=True`
...
```

---

_Comment by @zanieb on 2024-10-01 05:01_

I wonder if we use `strict=False` to avoid changing the semantics / behavior of the code? I think this is probably correct, but we should recommend adding `strict` not necessarily `strict=False` in the message?

---

_Label `rule` added by @zanieb on 2024-10-01 05:01_

---

_Comment by @mdbernard on 2024-10-01 05:04_

That also works! Just removing the autofix altogether and letting the user decide what they want. Based on personal experience I'd almost always want `strict=True`, but I see the counter-argument that it changes behavior, so it might not be a desirable autofix.

---

_Comment by @zanieb on 2024-10-01 05:09_

I don't think we should remove the autofix entirely though, just change the message.

---

_Referenced in [astral-sh/ruff#13656](../../astral-sh/ruff/pulls/13656.md) on 2024-10-07 10:04_

---

_Closed by @AlexWaygood on 2024-10-07 11:56_

---

_Referenced in [rapidsai/cudf#15835](../../rapidsai/cudf/issues/15835.md) on 2025-08-01 19:15_

---
