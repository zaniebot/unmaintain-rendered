---
number: 5604
title: Unstable formatting with expression comments in mypyc
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-07-08T07:18:53Z
updated_at: 2023-07-16T18:12:36Z
url: https://github.com/astral-sh/ruff/issues/5604
synced_at: 2026-01-07T13:12:15-06:00
---

# Unstable formatting with expression comments in mypyc

---

_Issue opened by @konstin on 2023-07-08 07:18_

The formatter formats [mypy/mypy/stubgenc.py:637-648](https://github.com/python/mypy/blob/6cd8c00983a294b4b142ee0f01e91912363d3450/mypy/stubgenc.py#L637-L648) different on the second pass.

Minimized example:

```python
f = a in (
    "b",
    "c",
) or d(  # comment
    e
)
```

```
$ cargo run --bin ruff_dev -- format-dev --stability-check --format full target/minirepo
Reformatting the formatted code a second time resulted in formatting changes.
---
--- Formatted once
+++ Formatted twice
@@ -4,5 +4,5 @@
         "b",
         "c",
     )
-    or d(e)  # comment
-)
+    or d(e)
+)  # comment
---

Formatted once:
---
f = (
    a
    in (
        "b",
        "c",
    )
    or d(e)  # comment
)
---

Formatted twice:
---
f = (
    a
    in (
        "b",
        "c",
    )
    or d(e)
)  # comment
---
```

---

_Label `formatter` added by @konstin on 2023-07-08 07:18_

---

_Comment by @konstin on 2023-07-16 18:12_

This has been fixed on main

---

_Closed by @konstin on 2023-07-16 18:12_

---
