---
number: 20883
title: "False positive with `UP007` and `ForwardRef` on python 3.10"
type: issue
state: open
author: Liam-DeVoe
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-15T06:09:37Z
updated_at: 2025-10-16T13:50:03Z
url: https://github.com/astral-sh/ruff/issues/20883
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive with `UP007` and `ForwardRef` on python 3.10

---

_Issue opened by @Liam-DeVoe on 2025-10-15 06:09_

### Summary

Ruff reports `UP007` when running `ruff check --target-version py310 '/Users/tybug/Desktop/sandbox4.py'` on the following:

```python
from typing import ForwardRef, Union

def f(x: Union[int, ForwardRef("A")]):
    pass
```

However, this is unsound on 3.10, because `int | ForwardRef("A")` errors. This is sound on 3.11+.

I know you're not really supposed to instantiate `ForwardRef` yourself, so you might consider this a wontfix. This arose for me when manually creating a signature which includes a forward ref in one of its types. See https://github.com/HypothesisWorks/hypothesis/pull/4558#discussion_r2402907294 for context.

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Referenced in [HypothesisWorks/hypothesis#4558](../../HypothesisWorks/hypothesis/pulls/4558.md) on 2025-10-15 06:10_

---

_Comment by @ntBre on 2025-10-16 13:49_

Thanks for the report! This seems related to https://github.com/astral-sh/ruff/issues/19144. I also found https://github.com/python/cpython/issues/89652.

---

_Label `rule` added by @ntBre on 2025-10-16 13:50_

---

_Label `needs-decision` added by @ntBre on 2025-10-16 13:50_

---
