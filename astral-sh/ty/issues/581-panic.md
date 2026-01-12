```yaml
number: 581
title: "[panic]"
type: issue
state: closed
author: rindlow
labels: []
assignees: []
created_at: 2025-06-04T08:31:16Z
updated_at: 2025-06-04T08:38:03Z
url: https://github.com/astral-sh/ty/issues/581
synced_at: 2026-01-12T15:54:23Z
```

# [panic]

---

_@rindlow_

Hi, I was testing ty when it panicked:

error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/2b51887/src/function/execute.rs:212:25 when checking `/Users/henrik/Source/python/medieval/ty-bug.py`: `infer_scope_types(Id(c01)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "ty-bug.py"]
info: Backtrace:
   0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __mh_execute_header
  30: __mh_execute_header
  31: __mh_execute_header
  32: __pthread_cond_wait

info: query stacktrace:
   0: check_types(Id(800))
             at crates/ty_python_semantic/src/types.rs:86


A minimal code example to cause the panic attached

//Henrik

[ty-bug.py.txt](https://github.com/user-attachments/files/20587930/ty-bug.py.txt)



---

_Comment by @sharkdp on 2025-06-04 08:38_

Thank you for reporting this.

This is the same issue as #256, see also the second item in https://github.com/astral-sh/ty/issues/445.

---

_Closed by @sharkdp on 2025-06-04 08:38_

---
