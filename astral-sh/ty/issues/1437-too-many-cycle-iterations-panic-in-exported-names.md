```yaml
number: 1437
title: "\"Too many cycle iterations\" panic in `exported_names` query"
type: issue
state: closed
author: f-schnabel
labels:
  - fatal
  - imports
assignees: []
created_at: 2025-10-25T23:42:38Z
updated_at: 2025-10-27T06:59:45Z
url: https://github.com/astral-sh/ty/issues/1437
synced_at: 2026-01-12T15:54:25Z
```

# "Too many cycle iterations" panic in `exported_names` query

---

_@f-schnabel_

```
error[panic]: Panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\d38145c\src\function\execute.rs:417:17 when checking `...pyrekordbox\mysettings\structs.py`: `exported_names(Id(c1b)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Version: 0.0.1-alpha.24 (1fee7da8b 2025-10-23)
info: Args: ["ty", "check", ".\\pyrekordbox\\mysettings\\structs.py"]
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
  33: <unknown>
  34: <unknown>
  35: <unknown>
  36: <unknown>
  37: <unknown>
  38: <unknown>
  39: <unknown>
  40: <unknown>
  41: <unknown>
  42: BaseThreadInitThunk
  43: RtlUserThreadStart

info: query stacktrace:
   0: semantic_index(Id(c13))
             at crates\ty_python_semantic\src\semantic_index.rs:54
   1: global_scope(Id(c13))
             at crates\ty_python_semantic\src\semantic_index.rs:183
   2: Type < 'db >::member_lookup_with_policy_(Id(2c00))
             at crates\ty_python_semantic\src\types.rs:783
   3: infer_definition_types(Id(1400))
             at crates\ty_python_semantic\src\types\infer.rs:90
   4: infer_scope_types(Id(1000))
             at crates\ty_python_semantic\src\types\infer.rs:70
   5: check_file_impl(Id(c00))
             at crates\ty_project\src\lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

File in question is here: https://github.com/dylanljones/pyrekordbox/blob/master/pyrekordbox/mysettings/structs.py
It is using https://construct.readthedocs.io/en/latest/ as a dependency

---

_Label `fatal` added by @AlexWaygood on 2025-10-26 14:26_

---

_Label `imports` added by @AlexWaygood on 2025-10-26 14:26_

---

_Renamed from "[panic]" to ""Too many cycle iterations" panic in `exported_names` query" by @AlexWaygood on 2025-10-26 14:27_

---

_Comment by @MichaReiser on 2025-10-27 06:59_

Thank you. I think this is the same as https://github.com/astral-sh/ty/issues/444

---

_Closed by @MichaReiser on 2025-10-27 06:59_

---
