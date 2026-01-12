```yaml
number: 1023
title: "\"too many cycle iterations\" panic on recursive type alias definitions"
type: issue
state: closed
author: aldanor
labels: []
assignees: []
created_at: 2025-08-17T02:09:56Z
updated_at: 2025-08-17T07:29:01Z
url: https://github.com/astral-sh/ty/issues/1023
synced_at: 2026-01-12T15:54:24Z
```

# "too many cycle iterations" panic on recursive type alias definitions

---

_@aldanor_

### Summary

```python
T = dict[str, int | "T"]
```

```
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `test.py`: `infer_definition_types(Id(1800)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.18 (d697cc092 2025-08-14)
info: Args: ["ty", "check", "test.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:136
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:527


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

### Version

0.0.1a18

---

_Closed by @AlexWaygood on 2025-08-17 07:29_

---
