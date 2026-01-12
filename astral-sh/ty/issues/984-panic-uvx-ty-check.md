```yaml
number: 984
title: "[panic] uvx ty check"
type: issue
state: closed
author: Kumzy
labels:
  - fatal
assignees: []
created_at: 2025-08-14T10:46:42Z
updated_at: 2025-08-15T15:38:46Z
url: https://github.com/astral-sh/ty/issues/984
synced_at: 2026-01-12T15:54:24Z
```

# [panic] uvx ty check

---

_@Kumzy_

Creating an issue for a panic of ty check

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/Users/x/Documents/Sources/litestar/litestar/security/session_auth/auth.py`: `infer_definition_types(Id(3e19)): execute: too many cycle iterations`
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.18 (d697cc092 2025-08-14)
info: Args: ["ty", "check", "litestar"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(1b80d))
             at crates/ty_python_semantic/src/place.rs:688
   1: Type < 'db >::member_lookup_with_policy_(Id(1b40c))
             at crates/ty_python_semantic/src/types.rs:630
   2: infer_definition_types(Id(3c0c))
             at crates/ty_python_semantic/src/types/infer.rs:165
   3: infer_scope_types(Id(2c00))
             at crates/ty_python_semantic/src/types/infer.rs:136
   4: check_file_impl(Id(c6f))
             at crates/ty_project/src/lib.rs:527
```

Other same issues:

- ``litestar/litestar/testing/client/sync_client.py``: `infer_definition_types(Id(3e19)): execute: too many cycle iterations`
- ``litestar/litestar/middleware/session/__init__.py``: `infer_definition_types(Id(3e0b)): execute: too many cycle iterations`
- ``litestar/litestar/cli/commands/sessions.py``: `infer_definition_types(Id(3e0b)): execute: too many cycle iterations`
- ``litestar/litestar/testing/client/async_client.py``: `infer_definition_types(Id(3e19)): execute: too many cycle iterations`

Project with the files: https://github.com/litestar-org/litestar
Branch: main

---

_Label `fatal` added by @AlexWaygood on 2025-08-14 10:56_

---

_Comment by @Kumzy on 2025-08-14 12:10_

The backtrace with the  env variable

```sh
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
  80: __mh_execute_header
  81: __mh_execute_header
  82: __mh_execute_header
  83: __mh_execute_header
  84: __mh_execute_header
  85: __mh_execute_header
  86: __mh_execute_header
  87: __mh_execute_header
  88: __mh_execute_header
  89: __mh_execute_header
  90: __mh_execute_header
  91: __mh_execute_header
  92: __mh_execute_header
  93: __mh_execute_header
  94: __pthread_joiner_wake
```


---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:30_

---

_Closed by @carljm on 2025-08-15 15:38_

---
