```yaml
number: 71
title: ty is slow for large tuples
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - testing
assignees: []
created_at: 2025-05-05T13:44:31Z
updated_at: 2025-08-15T15:03:12Z
url: https://github.com/astral-sh/ty/issues/71
synced_at: 2026-01-10T02:06:24Z
```

# ty is slow for large tuples

---

_Issue opened by @MichaReiser on 2025-05-05 13:44_

The file mentioned in https://github.com/astral-sh/ruff/issues/17572 takes very long to type check. It passes immediately if I shrink the large tuple.

---

_Label `performance` added by @MichaReiser on 2025-05-05 13:44_

---

_Comment by @AlexWaygood on 2025-05-05 14:22_

Type checkers often struggle with very large nested collections (which unfortunately are produced fairly often by code-generation tools). See e.g.:
- https://github.com/python/mypy/issues/14970
- https://github.com/python/mypy/issues/14636

(Not arguing that we shouldn't try to do better here than other type checkers -- we should!)

---

_Added to milestone `ty beta` by @MichaReiser on 2025-05-07 15:04_

---

_Label `testing` added by @MichaReiser on 2025-05-07 15:04_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-13 18:48_

---

_Unassigned @sharkdp by @sharkdp on 2025-05-13 18:53_

---

_Comment by @erictraut on 2025-07-23 21:37_

I had a similar problem in pyright when dealing with very large tuple literal expressions. 

My solution was to add [a limit](https://github.com/microsoft/pyright/blob/8b02aa84ec916697e76d5ee69299a89d555ec98c/packages/pyright-internal/src/analyzer/tuples.ts#L53) to the number of tuple entries. If there is no inference context (i.e. it's not assigned to a variable or parameter with a declared type) and the number of entries exceeds the limit (currently 256), then the type of the expression is inferred to be `tuple[Unknown, ...]`. That behavior isn't ideal, but I figured it was preferable to long hangs. 

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:03_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:03_

---
