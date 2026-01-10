```yaml
number: 1347
title: "[panic] on recursive tuple type"
type: issue
state: closed
author: cgravill
labels: []
assignees: []
created_at: 2025-10-13T13:52:18Z
updated_at: 2025-10-13T14:01:38Z
url: https://github.com/astral-sh/ty/issues/1347
synced_at: 2026-01-10T02:06:25Z
```

# [panic] on recursive tuple type

---

_Issue opened by @cgravill on 2025-10-13 13:52_

```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/badger/badger_badger_badger.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(6001)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.22
info: Args: ["ty", "check", "badger/badger_badger_badger.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: TupleType < 'db >::to_class_type_(Id(7800))
             at crates/ty_python_semantic/src/types/tuple.rs:149
   1: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:69
   2: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

As a minimal reproduction:

```
type A[T] = type[T] | tuple[A[T]]
```

I did a search and some panic reports seemed related but that they were addressed by https://github.com/astral-sh/ruff/pull/20598 which I think my version has in it

---

_Comment by @AlexWaygood on 2025-10-13 14:01_

Thanks for the great repro! I'll fold this into https://github.com/astral-sh/ty/issues/256, but I'll add this example as a comment on that issue

---

_Closed by @AlexWaygood on 2025-10-13 14:01_

---
