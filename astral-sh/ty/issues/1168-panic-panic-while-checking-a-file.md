```yaml
number: 1168
title: "[panic] panic while checking a file"
type: issue
state: closed
author: bellini666
labels: []
assignees: []
created_at: 2025-09-11T00:33:16Z
updated_at: 2025-09-11T00:41:47Z
url: https://github.com/astral-sh/ty/issues/1168
synced_at: 2026-01-12T15:54:24Z
```

# [panic] panic while checking a file

---

_@bellini666_

Got those 2 panics while testing ty on https://github.com/strawberry-graphql/strawberry-django (which is currently using `pyright` as a typing linter)

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/Users/bellini/dev/strawberry-django/strawberry_django/utils/inspect.py`: `infer_definition_types(Id(5ca10)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.20 (f41f00af1 2025-09-03)
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
  55: __pthread_cond_wait

info: query stacktrace:
   0: place_by_id(Id(1391c))
             at crates/ty_python_semantic/src/place.rs:702
   1: Type < 'db >::member_lookup_with_policy_(Id(1010d))
             at crates/ty_python_semantic/src/types.rs:715
   2: infer_definition_types(Id(548e2))
             at crates/ty_python_semantic/src/types/infer.rs:174
   3: infer_scope_types(Id(3670))
             at crates/ty_python_semantic/src/types/infer.rs:145
   4: check_file_impl(Id(2410))
             at crates/ty_project/src/lib.rs:522


error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/Users/bellini/dev/strawberry-django/strawberry_django/utils/pyutils.py`: `infer_definition_types(Id(5ca10)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.20 (f41f00af1 2025-09-03)
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
  33: __pthread_cond_wait

info: query stacktrace:
   0: infer_scope_types(Id(c6f7))
             at crates/ty_python_semantic/src/types/infer.rs:145
   1: check_file_impl(Id(240d))
             at crates/ty_project/src/lib.rs:522
```

Panic files are:
- https://github.com/strawberry-graphql/strawberry-django/blob/main/strawberry_django/utils/inspect.py
- https://github.com/strawberry-graphql/strawberry-django/blob/main/strawberry_django/utils/pyutils.py

---

_Renamed from "[panic]" to "[panic] panic while checking a file" by @bellini666 on 2025-09-11 00:33_

---

_Comment by @carljm on 2025-09-11 00:41_

Thanks for the report!

---

_Closed by @carljm on 2025-09-11 00:41_

---
