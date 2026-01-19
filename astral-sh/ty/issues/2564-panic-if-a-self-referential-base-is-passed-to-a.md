```yaml
number: 2564
title: "Panic if a self-referential base is passed to a three-argument `type()` call"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - fatal
assignees: []
created_at: 2026-01-19T15:31:23Z
updated_at: 2026-01-19T15:37:06Z
url: https://github.com/astral-sh/ty/issues/2564
synced_at: 2026-01-19T16:26:48Z
```

# Panic if a self-referential base is passed to a three-argument `type()` call

---

_@AlexWaygood_

ty currently panics on this:

```py
X = type("X", (tuple["X | None"],), {})
```

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/9860ff6/src/function/execute.rs:743:9 when checking `/Users/alexw/dev/ruff/foo.py`: `infer_definition_types(Id(1400)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.13+65 (bffc95873 2026-01-19)
info: Args: ["target/debug/ty", "check", "foo.py", "--python-version=3.14", "--ignore=undefined-reveal"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:554
```

After https://github.com/astral-sh/ruff/pull/22718 has landed, things like this will also panic:

```py
from typing import NamedTuple

X = type("X", (NamedTuple("NT", [("field", "X | int")]),), {})
```

The fix is probably to defer inference of the `bases` argument in "non-dangling" three-argument `type()` calls, the same way as we defer inference of the bases list in regular `class` statements. "Non-dangling" means three-argument `type()` calls where the call is immediately assigned to a value, such as the above assignment to `X`.

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-19 15:31_

---

_Label `fatal` added by @AlexWaygood on 2026-01-19 15:31_

---

_Label `bug` added by @AlexWaygood on 2026-01-19 15:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-19 15:36_

---
