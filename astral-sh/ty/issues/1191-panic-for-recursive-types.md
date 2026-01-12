```yaml
number: 1191
title: "[panic] for recursive types"
type: issue
state: closed
author: chrreisinger
labels: []
assignees: []
created_at: 2025-09-16T10:19:43Z
updated_at: 2025-09-16T11:01:03Z
url: https://github.com/astral-sh/ty/issues/1191
synced_at: 2026-01-12T15:54:24Z
```

# [panic] for recursive types

---

_@chrreisinger_

Hi,

I encountered this crash:

```
ty check ./test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                                                                                                                             error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/Users/christian/source/ganymed/fr1_ws1/test.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(7800)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.20 (f41f00af1 2025-09-03)
info: Args: ["ty", "check", "./test.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: ClassLiteral < 'db >::try_mro_(Id(3801))
             at crates/ty_python_semantic/src/types/class.rs:1377
   1: ClassLiteral < 'db >::try_metaclass_(Id(3406))
             at crates/ty_python_semantic/src/types/class.rs:1377
   2: code_generator_of_class(Id(3406))
             at crates/ty_python_semantic/src/types/class.rs:191
   3: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:145
   4: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
```

Reproducer:

```
from collections.abc import Sequence

type TYPE = Foo | Sequence[TYPE]


class Foo[T = TYPE]:
    bitfields: Sequence[T]
```

Thx!

---

_Comment by @AlexWaygood on 2025-09-16 10:30_

Thanks! Please see #256

---

_Closed by @AlexWaygood on 2025-09-16 10:30_

---

_Comment by @AlexWaygood on 2025-09-16 10:37_

But this repro is interesting because it involves recursive PEP-695 aliases, which should _generally_ be supported now. I'll add it as a comment to #256

---

_Comment by @chrreisinger on 2025-09-16 11:01_

Thx!

---
