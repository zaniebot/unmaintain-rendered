```yaml
number: 1848
title: "panic: `place_by_id... execute: too many cycle iterations`"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-11T00:45:33Z
updated_at: 2025-12-23T21:22:55Z
url: https://github.com/astral-sh/ty/issues/1848
synced_at: 2026-01-10T01:56:41Z
```

# panic: `place_by_id... execute: too many cycle iterations`

---

_Issue opened by @correctmost on 2025-12-11 00:45_

### Summary

ty crashes after ~20 seconds when checking this fuzzed code:

```python
T = tuple[int, 'U']

class C(set['U']):
    pass

type U = T | C
```

```
error[panic]: Panicked at /home/user/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:321:21 when checking `/home/user/a.py`: `place_by_id(Id(2c06)): execute: too many cycle iterations`
```

### Version

35e23aa w/ astral-sh/ruff@29bf2cd2

---

_Label `fuzzer` added by @carljm on 2025-12-11 01:44_

---

_Label `fatal` added by @carljm on 2025-12-11 01:44_

---

_Added to milestone `Stable` by @carljm on 2025-12-11 01:44_

---

_Label `bug` added by @AlexWaygood on 2025-12-11 02:31_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-13 16:08_

---

_Closed by @carljm on 2025-12-23 21:22_

---
