```yaml
number: 899
title: "`type: type = …` fails with deferred annotations"
type: issue
state: closed
author: WyattBlue
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-07-26T21:05:39Z
updated_at: 2025-12-07T15:58:12Z
url: https://github.com/astral-sh/ty/issues/899
synced_at: 2026-01-12T15:54:24Z
```

# `type: type = …` fails with deferred annotations

---

_@WyattBlue_


```python
# test.py
from __future__ import annotations


class MyClass:
    type: type = str
```

Running:
```
ty check test.py
```
Causes a panic:

```
ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/dba66f1/src/function/execute.rs:212:25 when checking `/Users/wyattblue/projects/auto-editor/test.py`: `infer_definition_types(Id(1401)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.16 (c452e53a0 2025-07-25)
info: Args: ["ty", "check", "test.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:134
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:514


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Label `bug` added by @sharkdp on 2025-07-28 07:30_

---

_Label `fatal` added by @sharkdp on 2025-07-28 07:30_

---

_Comment by @sharkdp on 2025-07-28 07:32_

Thank you for reporting this.

This falls into the same category as #256, but is not related to generics or type aliases, so it might make sense to track this separately.

---

_Renamed from "[panic] `type: type = ` fails when annotations is enabled" to "`type: type = …` fails with deferred annotations" by @sharkdp on 2025-07-28 07:32_

---

_Closed by @AlexWaygood on 2025-12-07 15:58_

---
