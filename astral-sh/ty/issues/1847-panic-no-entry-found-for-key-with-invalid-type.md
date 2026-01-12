```yaml
number: 1847
title: "panic: `no entry found for key` with invalid type expression"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-10T21:35:37Z
updated_at: 2025-12-11T08:40:21Z
url: https://github.com/astral-sh/ty/issues/1847
synced_at: 2026-01-12T15:54:25Z
```

# panic: `no entry found for key` with invalid type expression

---

_@correctmost_

### Summary

ty crashes when checking this fuzzed code:

```python
_: "a*(_ for _ in _)"
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:7815:14 when checking `/home/user/a.py`: `no entry found for key`
```

### Version

35e23aa w/ astral-sh/ruff@1b44d7e

---

_Label `bug` added by @AlexWaygood on 2025-12-10 21:37_

---

_Label `fatal` added by @AlexWaygood on 2025-12-10 21:37_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-10 21:37_

---

_Added to milestone `Stable` by @carljm on 2025-12-10 21:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-11 00:55_

---

_Closed by @sharkdp on 2025-12-11 08:40_

---
