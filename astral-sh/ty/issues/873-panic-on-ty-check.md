```yaml
number: 873
title: "[panic] On `ty check`"
type: issue
state: closed
author: stephenprater
labels:
  - needs-mre
  - fatal
assignees: []
created_at: 2025-07-22T16:10:20Z
updated_at: 2025-07-23T07:10:19Z
url: https://github.com/astral-sh/ty/issues/873
synced_at: 2026-01-12T15:54:24Z
```

# [panic] On `ty check`

---

_@stephenprater_

### Summary

This is probably not super helpful because I can't share the actual Python that crashes it - but here's the backtrace:

```

Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/fc00eba/src/function/execute.rs:212:25 when checking `<my_python_file.py>`: `infer_definition_types(Id(a490)): execute: too many cycle iterations`

info: Platform: macos aarch64
info: Args: ["ty", "check", "support_agent"]
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
  32: __mh_execute_header
  33: __mh_execute_header
  34: __mh_execute_header
  35: __mh_execute_header
  36: __mh_execute_header
  37: __mh_execute_header
  38: __mh_execute_header
  39: __mh_execute_header
  40: __mh_execute_header
  41: __mh_execute_header
  42: __mh_execute_header
  43: __mh_execute_header
  44: __mh_execute_header
  45: __mh_execute_header
  46: __mh_execute_header
  47: __mh_execute_header
  48: __mh_execute_header
  49: __mh_execute_header
  50: __mh_execute_header
  51: __mh_execute_header
  52: __mh_execute_header
  53: __mh_execute_header
  54: __mh_execute_header
  55: __mh_execute_header
  56: __mh_execute_header
  57: __mh_execute_header
  58: __mh_execute_header
  59: __mh_execute_header
  60: __mh_execute_header
  61: __mh_execute_header
  62: __mh_execute_header
  63: __mh_execute_header
  64: __mh_execute_header
  65: __mh_execute_header
  66: __mh_execute_header
  67: __mh_execute_header
  68: __mh_execute_header
  69: __mh_execute_header
  70: __mh_execute_header
  71: __mh_execute_header
  72: __mh_execute_header
  73: __mh_execute_header
  74: __mh_execute_header
  75: __mh_execute_header
  76: __mh_execute_header
  77: __mh_execute_header
  78: __mh_execute_header
  79: __mh_execute_header
  80: __pthread_cond_wait

info: query stacktrace:
   0: infer_definition_types(Id(cc98))
             at crates/ty_python_semantic/src/types/infer.rs:159
   1: Type < 'db >::class_member_with_policy_(Id(25876))
             at crates/ty_python_semantic/src/types.rs:590
   2: Type < 'db >::member_lookup_with_policy_(Id(1c8c5))
             at crates/ty_python_semantic/src/types.rs:590
   3: infer_scope_types(Id(3d9d))
             at crates/ty_python_semantic/src/types/infer.rs:130
   4: check_types(Id(c30))
             at crates/ty_python_semantic/src/types.rs:91
```

It works when running in `server` mode - only when running `check` on the command line does it panic.

### Version

ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)

---

_Label `needs-mre` added by @AlexWaygood on 2025-07-22 16:16_

---

_Label `fatal` added by @AlexWaygood on 2025-07-22 16:16_

---

_Comment by @sharkdp on 2025-07-23 07:10_

Thank you very much for reporting this. From the looks of it, it is very likely that this is related to https://github.com/astral-sh/ty/issues/256. I hope it's fine if I close this under that assumption.

---

_Closed by @sharkdp on 2025-07-23 07:10_

---
