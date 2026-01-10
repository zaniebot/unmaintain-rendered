```yaml
number: 830
title: "[panic] dependency graph cycle when querying PEP695TypeAliasType"
type: issue
state: closed
author: floriankapaun
labels: []
assignees: []
created_at: 2025-07-16T13:28:48Z
updated_at: 2025-07-16T13:29:12Z
url: https://github.com/astral-sh/ty/issues/830
synced_at: 2026-01-10T02:07:36Z
```

# [panic] dependency graph cycle when querying PEP695TypeAliasType

---

_Issue opened by @floriankapaun on 2025-07-16 13:28_

```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `/workspace/.../foo.py`: `infer_scope_types(Id(1001)): execute: too many cycle iterations`
```

`foo.py` for reproduction:
```python
type ObjectWithDatetimes = list["ObjectWithDatetimes"]
```

Info:
```
info: Platform: linux x86_64
info: Args: ["/home/rfa/.pyenv/versions/3.13.3/bin/ty", "check", "app/utils/datetime_utils.py"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>

info: query stacktrace:
   0: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:91
```



---

_Comment by @floriankapaun on 2025-07-16 13:29_

duplicate of https://github.com/astral-sh/ty/issues/256

---

_Closed by @floriankapaun on 2025-07-16 13:29_

---
