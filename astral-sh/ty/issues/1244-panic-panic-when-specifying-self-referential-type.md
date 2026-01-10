```yaml
number: 1244
title: "[panic] Panic when specifying self referential Type Variable."
type: issue
state: closed
author: sakosha
labels:
  - fatal
assignees: []
created_at: 2025-09-24T10:23:01Z
updated_at: 2025-09-24T14:36:53Z
url: https://github.com/astral-sh/ty/issues/1244
synced_at: 2026-01-10T02:06:25Z
```

# [panic] Panic when specifying self referential Type Variable.

---

_Issue opened by @sakosha on 2025-09-24 10:23_

I tried using `typing.Self` as a TypeVar but got errors from mypy: `error: Self type is only allowed in annotations within class definition`

```python
from typing import Any
from typing import Protocol
from typing import TypeVar

from contextlib import AbstractAsyncContextManager

UoWType_co = TypeVar('UoWType_co', bound='UnitOfWork[Any]', covariant=True)


class UnitOfWork(AbstractAsyncContextManager[UoWType_co], Protocol):
    async def commit(self) -> None: ...

    async def rollback(self) -> None: ...
```

actual output:
```
ty check bugreport.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                          error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/sanzhar/PythonProjects/assp/bugreport.py`: `infer_definition_types(Id(1404)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "bugreport.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:68
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

expected output:
`All checks passed!`

full backtrace:
```
RUST_BACKTRACE=1 ty check bugreport.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                          error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/sanzhar/PythonProjects/assp/bugreport.py`: `infer_definition_types(Id(1404)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "bugreport.py"]
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
  40: start_thread
             at ./nptl/pthread_create.c:447:8
  41: clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78:0

info: query stacktrace:
   0: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:68
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
not sure why it's being shown as unknown.

---

_Comment by @sakosha on 2025-09-24 10:32_

smaller repro:
```python
from typing import Any
from typing import TypeVar
from abc import ABC

ChildT = TypeVar('ChildT', bound='Child[Any]')


class GenericABC[Child_co](ABC):
    pass


class Child(GenericABC[ChildT]):
    pass
```

---

_Comment by @sakosha on 2025-09-24 10:35_

Most likely dup/variation of #256 and https://github.com/astral-sh/ty/issues/256#issuecomment-3140776680, but as I see https://github.com/astral-sh/ruff/pull/20359 did not fix either of those.

---

_Label `fatal` added by @AlexWaygood on 2025-09-24 14:36_

---

_Comment by @AlexWaygood on 2025-09-24 14:36_

Yes, I think we can close this as a variation of https://github.com/astral-sh/ty/issues/256#issuecomment-3140776680. But thanks for the report!

---

_Closed by @AlexWaygood on 2025-09-24 14:36_

---
