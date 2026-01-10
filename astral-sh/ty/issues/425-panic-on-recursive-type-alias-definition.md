```yaml
number: 425
title: Panic on recursive type alias definition
type: issue
state: closed
author: Akuli
labels: []
assignees: []
created_at: 2025-05-16T15:01:56Z
updated_at: 2025-05-16T16:23:17Z
url: https://github.com/astral-sh/ty/issues/425
synced_at: 2026-01-10T02:34:09Z
```

# Panic on recursive type alias definition

---

_Issue opened by @Akuli on 2025-05-16 15:01_

### Summary

I have the following file `a.py`:

```python
from typing import Sequence, TypeAlias
_MenuContent: TypeAlias = Sequence[tuple[str, "_MenuContent"]]
```

When I run `cargo run -p ty -- check a.py`, I get:

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/ty check a.py`
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at /home/akuli/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/7edce6e/src/function/execute.rs:209:25 when checking `/home/akuli/sourcestuff/ruff/a.py`: `infer_definition_types(Id(1002)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["target/debug/ty", "check", "a.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(c00))
             at crates/ty_python_semantic/src/types/infer.rs:121
   1: check_types(Id(800))
             at crates/ty_python_semantic/src/types.rs:84


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

This has been discussed previously in astral-sh/ruff#14672, and the panic does go away if I remove `Sequence`.

(As the message says, I don't expect the type checker to be usable yet, but I thought I might as well report this.)

### Version

commit 1ba56b4bc65442c40d7d6f2873367380d37241a8 (latest main)

---

_Renamed from "[red-knot] Panic on recursive type alias definition" to "Panic on recursive type alias definition" by @ntBre on 2025-05-16 15:21_

---

_Closed by @AlexWaygood on 2025-05-16 16:22_

---

_Reopened by @AlexWaygood on 2025-05-16 16:22_

---

_Closed by @AlexWaygood on 2025-05-16 16:22_

---

_Comment by @AlexWaygood on 2025-05-16 16:23_

Thanks! I'm pretty sure that this is another instance of #256, since the reproducer is very similar and the panic message is the same

---
