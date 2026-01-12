```yaml
number: 1865
title: "panic: `no entry found for key` with stringified set comprehension in type expression"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-12T13:50:31Z
updated_at: 2025-12-16T03:31:05Z
url: https://github.com/astral-sh/ty/issues/1865
synced_at: 2026-01-12T15:54:26Z
```

# panic: `no entry found for key` with stringified set comprehension in type expression

---

_@correctmost_

### Summary

ty crashes when checking this fuzzed code:

```python
x: "f'{1 if 1 else 1}'"
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:8187:28 when checking `/home/user/a.py`: `no entry found for key`
```

### Version

5e31bc93 w/ astral-sh/ruff@bc8efa2fd86

---

_Label `bug` added by @AlexWaygood on 2025-12-12 13:52_

---

_Label `fatal` added by @AlexWaygood on 2025-12-12 13:52_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-12 13:52_

---

_Added to milestone `Stable` by @carljm on 2025-12-12 16:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 19:58_

---

_Closed by @charliermarsh on 2025-12-16 03:31_

---
