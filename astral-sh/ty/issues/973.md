```yaml
number: 973
title: "[panic] `infer_definition_types(Id(2aed7)): execute: too many cycle iterations`"
type: issue
state: closed
author: sam-atkins
labels:
  - needs-mre
  - fatal
assignees: []
created_at: 2025-08-12T11:01:25Z
updated_at: 2025-08-12T13:37:56Z
url: https://github.com/astral-sh/ty/issues/973
synced_at: 2026-01-10T02:06:24Z
```

# [panic] `infer_definition_types(Id(2aed7)): execute: too many cycle iterations`

---

_Issue opened by @sam-atkins on 2025-08-12 11:01_

```
uvx ty check
Installed 1 package in 3ms
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 11/11 files                                                                                        error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d66fe33/src/function/execute.rs:202:25 when checking `/Users/sam/work/poc/execution-data-poc/tests/test_app.py`: `infer_definition_types(Id(2aed7)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.17 (1712284c8 2025-08-06)
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(12cc9))
             at crates/ty_python_semantic/src/place.rs:644
   1: Type < 'db >::member_lookup_with_policy_(Id(10882))
             at crates/ty_python_semantic/src/types.rs:614
   2: infer_definition_types(Id(4026))
             at crates/ty_python_semantic/src/types/infer.rs:163
   3: place_by_id(Id(12cc8))
             at crates/ty_python_semantic/src/place.rs:644
   4: infer_deferred_types(Id(4035))
             at crates/ty_python_semantic/src/types/infer.rs:203
   5: FunctionType < 'db >::signature_(Id(21029))
             at crates/ty_python_semantic/src/types/function.rs:614
   6: infer_expression_types(Id(4c00))
             at crates/ty_python_semantic/src/types/infer.rs:244
   7: infer_definition_types(Id(4002))
             at crates/ty_python_semantic/src/types/infer.rs:163
   8: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:134
   9: check_file_impl(Id(c08))
             at crates/ty_project/src/lib.rs:529


Found 1 diagnostic
```

Above is the error from stdout. Can provide more info if required, just let me know.


---

_Comment by @AlexWaygood on 2025-08-12 11:07_

This is most commonly caused by missing support for recursive type aliases and recursive generics (https://github.com/astral-sh/ty/issues/256). If that rings a bell then this is probably a duplicate of that issue. If not, we'll need to see a snippet of Python code that reliably reproduces the error in order to debug this :-) Ideally a self-contained snippet, but it's okay if it uses open-source third-party libraries.

---

_Label `needs-mre` added by @AlexWaygood on 2025-08-12 11:07_

---

_Label `fatal` added by @AlexWaygood on 2025-08-12 11:07_

---

_Comment by @sam-atkins on 2025-08-12 13:36_

I'd say it's a duplicate then 😄 

---

_Closed by @AlexWaygood on 2025-08-12 13:37_

---
