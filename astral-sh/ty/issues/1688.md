```yaml
number: 1688
title: "panic: assertion `left == right` failed"
type: issue
state: closed
author: correctmost
labels:
  - fatal
  - fuzzer
assignees: []
created_at: 2025-11-30T19:47:24Z
updated_at: 2025-12-05T13:07:49Z
url: https://github.com/astral-sh/ty/issues/1688
synced_at: 2026-01-10T01:56:40Z
```

# panic: assertion `left == right` failed

---

_Issue opened by @correctmost on 2025-11-30 19:47_

### Summary

ty crashes when checking this (fuzzed) code:

```python
x: int

type x[T] = x[T, U]
```

```
Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:11296:22 when checking `/home/user/foo.py`: `assertion `left == right` failed
  left: Some(Dynamic(Unknown))
 right: None`
```

### Version

ty 0.0.1-alpha.29

---

_Renamed from "panic: `assertion `left == right` failed" to "panic: assertion `left == right` failed" by @correctmost on 2025-11-30 19:47_

---

_Label `fatal` added by @AlexWaygood on 2025-11-30 21:55_

---

_Comment by @mtshiba on 2025-12-01 03:21_

This is addressed in astral-sh/ruff#21718.

---

_Added to milestone `Beta` by @carljm on 2025-12-02 19:05_

---

_Closed by @carljm on 2025-12-05 02:05_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:07_

---
