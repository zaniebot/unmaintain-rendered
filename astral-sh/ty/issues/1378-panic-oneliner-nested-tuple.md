```yaml
number: 1378
title: "panic oneliner: nested tuple"
type: issue
state: closed
author: hjalmarlucius
labels: []
assignees: []
created_at: 2025-10-17T08:53:41Z
updated_at: 2025-10-17T09:00:34Z
url: https://github.com/astral-sh/ty/issues/1378
synced_at: 2026-01-12T15:54:25Z
```

# panic oneliner: nested tuple

---

_@hjalmarlucius_

### Summary

This code causes panic: `type NestedTuple[T] = T | tuple[NestedTuple[T], ...]`

Command run: `uv run --with ty ty check -v typ.py`

```
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.13, platform: linux
INFO Indexed 1 file(s) in 0.002s
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `<FILEPATH>/typ.py`: `infer_scope_types(Id(1002)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.23
info: Args: ["ty", "check", "-v", "typ.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

### Version

ty 0.0.1-alpha.23

---

_Closed by @AlexWaygood on 2025-10-17 09:00_

---

_Comment by @AlexWaygood on 2025-10-17 09:00_

Thanks! Please see #256 :-)

---
