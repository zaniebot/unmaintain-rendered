```yaml
number: 1246
title: "[panic] importing Ontology from pronto package raises 'too many cycle iterations'"
type: issue
state: closed
author: jday1
labels:
  - fatal
assignees: []
created_at: 2025-09-24T14:56:17Z
updated_at: 2025-09-24T15:49:53Z
url: https://github.com/astral-sh/ty/issues/1246
synced_at: 2026-01-10T02:06:25Z
```

# [panic] importing Ontology from pronto package raises 'too many cycle iterations'

---

_Issue opened by @jday1 on 2025-09-24 14:56_

Repository:

https://github.com/althonos/pronto

Full stack trace:
```
(pronto_tmp) ➜  pronto git:(master) uvx ty check pronto/ontology.py                             
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                           error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/jamesday/Documents/repositories/pronto/pronto/ontology.py`: `infer_definition_types(Id(9c39)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "pronto/ontology.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(381b))
             at crates/ty_python_semantic/src/place.rs:706
   1: Type < 'db >::member_lookup_with_policy_(Id(3017))
             at crates/ty_python_semantic/src/types.rs:768
   2: infer_definition_types(Id(1412))
             at crates/ty_python_semantic/src/types/infer.rs:97
   3: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:68
   4: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Label `fatal` added by @AlexWaygood on 2025-09-24 15:00_

---

_Closed by @carljm on 2025-09-24 15:49_

---
