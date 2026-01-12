```yaml
number: 525
title: "[panic] too many cycle iterations"
type: issue
state: closed
author: poneill
labels:
  - bug
  - needs-mre
  - fatal
assignees: []
created_at: 2025-05-27T16:47:09Z
updated_at: 2025-08-15T12:08:57Z
url: https://github.com/astral-sh/ty/issues/525
synced_at: 2026-01-12T15:54:23Z
```

# [panic] too many cycle iterations

---

_@poneill_

Sorry if this is unhelpful-- I can't paste source because corporate code, but I got the following panic message with `ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)` and was asked to post it:
```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/4818b15/src/function/execute.rs:209:25 when checking `/Users/pat/REDACTED.py`: `infer_definition_types(Id(6f602)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check"]
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
  78: __pthread_joiner_wake

info: query stacktrace:
   0: infer_definition_types(Id(6f60a))
             at crates/ty_python_semantic/src/types/infer.rs:149
   1: class_member_with_policy_(Id(29714))
             at crates/ty_python_semantic/src/types.rs:550
   2: member_lookup_with_policy_(Id(84fb7))
             at crates/ty_python_semantic/src/types.rs:550
   3: infer_scope_types(Id(1e22))
             at crates/ty_python_semantic/src/types/infer.rs:122
   4: check_types(Id(ddb))
             at crates/ty_python_semantic/src/types.rs:84
```


---

_Label `bug` added by @AlexWaygood on 2025-05-27 16:48_

---

_Label `needs-mre` added by @AlexWaygood on 2025-05-27 16:48_

---

_Label `fatal` added by @AlexWaygood on 2025-05-27 16:48_

---

_Comment by @AlexWaygood on 2025-05-27 16:51_

Thanks! Do you happen to know if any of the types you're using are self-referential or recursive types, by any chance? This is the same backtrace as https://github.com/astral-sh/ty/issues/256, so it may well be the same issue

---

_Comment by @MichaReiser on 2025-08-15 12:08_

Thank you for reporting this.

I'll close this issue as it isn't very actionable for us without an MRE and there are other issues that track similar reports.

---

_Closed by @MichaReiser on 2025-08-15 12:08_

---
