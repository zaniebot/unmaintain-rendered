```yaml
number: 1870
title: Memory allocation failure with large tuple operations
type: issue
state: open
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-12T22:23:13Z
updated_at: 2025-12-12T23:21:34Z
url: https://github.com/astral-sh/ty/issues/1870
synced_at: 2026-01-10T01:55:00Z
```

# Memory allocation failure with large tuple operations

---

_Issue opened by @correctmost on 2025-12-12 22:23_

### Summary

ty crashes when checking this fuzzed code:
```python
for a in ('a', 'b'):
    a + (a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a)
```

```
memory allocation of 2199023255552 bytes failed
```

### Version

ty 0.0.1a34

---

_Label `bug` added by @AlexWaygood on 2025-12-12 22:24_

---

_Label `fatal` added by @AlexWaygood on 2025-12-12 22:24_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-12 22:24_

---

_Comment by @MichaReiser on 2025-12-12 22:41_

Interesting. The tuple isn't even that large

---

_Added to milestone `Stable` by @carljm on 2025-12-12 23:15_

---

_Comment by @MeGaGiGaGon on 2025-12-12 23:21_

If you make the tuple long enough you get an actual panic, which might be useful for debugging this:
```
PS ~>echo ("for a in ('a', 'b'): a + (" + "a, " * 500 + ")") > issue.py; uvx ty check issue.py
error[panic]: Panicked at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb\library\core\src\iter\traits\iterator.rs:2027:9 when checking `C:\Users\GiGaGon\issue.py`: `capacity overflow`
...
info: query stacktrace:
   0: infer_scope_types(Id(1000))
             at crates\ty_python_semantic\src\types\infer.rs:69
   1: check_file_impl(Id(c00))
             at crates\ty_project\src\lib.rs:535
```
No full backtrace since I don't have a debug build and the playground has stopped giving stack traces for whatever reason :(

Something else that's fun, if the length of the `("a", "b")` tuple is `n`, and the length of the `a + (...)` tuple is `k`, the number in the memory allocation message is equal to `16*(n**k)`

---
