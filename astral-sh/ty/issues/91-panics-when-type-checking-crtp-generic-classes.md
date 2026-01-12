```yaml
number: 91
title: panics when type-checking CRTP generic classes
type: issue
state: closed
author: wence-
labels:
  - bug
assignees: []
created_at: 2025-05-07T14:29:57Z
updated_at: 2025-05-08T14:15:55Z
url: https://github.com/astral-sh/ty/issues/91
synced_at: 2026-01-12T15:54:22Z
```

# panics when type-checking CRTP generic classes

---

_@wence-_

### Summary

[I found a few similar looking issues which may have the same cause, but this particular instantiation seemed to be new.]

Aside: the requested web link to report a new issue: `https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D` does not resolve.

Consider:
```python
from typing import Any, Generic, TypeVar

T = TypeVar("T", bound="Node[Any]")

class Node(Generic[T]):
    members: tuple[T, ...]

class Concrete(Node["Concrete"]):
    ...

reveal_type(Concrete.members)
```

```
$ target/debug/ty check --python-version 3.13 bug.py
error: panic: Panicked at /usr/local/cargo/git/checkouts/salsa-e6f3bb7c2a062968/f78a641/src/function/execute.rs:210:25 when checking `/home/coder/doodles/python/pyright/what.py`: `infer_definition_types(Id(1003)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["target/debug/ty", "check", "bug.py", "--python-version", "3.13"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(c00))
             at crates/ty_python_semantic/src/types/infer.rs:119
   1: check_types(Id(800))
             at crates/ty_python_semantic/src/types.rs:83


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

A slightly different error (but presumably a similar root cause) occurs for the PEP695 version

```python
from typing import Any


class Node[T: "Node[Any]"]:
    members: tuple[T, ...]

class Concrete(Node["Concrete"]):
    ...

reveal_type(Concrete.members)
```

```
$ target/debug/ty check --python-version 3.13 bug.py
error: panic: Panicked at /usr/local/cargo/git/checkouts/salsa-e6f3bb7c2a062968/f78a641/src/function/fetch.rs:131:25 when checking `/home/coder/doodles/python/pyright/what.py`: `dependency graph cycle when querying pep695_generic_context_(Id(2c01)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(800)),
    infer_scope_types(Id(c00)),
    member_lookup_with_policy_(Id(200b)),
    explicit_bases_query_(Id(2c02)),
    infer_deferred_types(Id(1004)),
    pep695_generic_context_(Id(2c01)),
    infer_definition_types(Id(1001)),
    symbol_by_id(Id(280c)),
    use_def_map(Id(c00)),
    member_lookup_with_policy_(Id(200a)),
    imported_modules(Id(1812)),
    all_narrowing_constraints_for_expression(Id(2470)),
    member_lookup_with_policy_(Id(2009)),
    imported_modules(Id(1811)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["target/debug/ty", "check", "bug.py", "--python-version", "3.13"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:146
   1: pep695_generic_context_(Id(2c01))
             at crates/ty_python_semantic/src/types/class.rs:499
   2: infer_deferred_types(Id(1004))
             at crates/ty_python_semantic/src/types/infer.rs:184
   3: explicit_bases_query_(Id(2c02))
             at crates/ty_python_semantic/src/types/class.rs:499
   4: member_lookup_with_policy_(Id(200b))
             at crates/ty_python_semantic/src/types.rs:537
   5: infer_scope_types(Id(c00))
             at crates/ty_python_semantic/src/types/infer.rs:119
   6: check_types(Id(800))
             at crates/ty_python_semantic/src/types.rs:83


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

### Version

ty version => ty unknown; ruff version => ruff 0.11.8+84 (3dedd70a9 2025-05-07)

---

_Comment by @carljm on 2025-05-07 14:32_

Thanks for the report! Yes, this is a known issue, we're considering possible solutions.

---

_Comment by @carljm on 2025-05-07 14:33_

(The bug-reporting link will be live soon.)

---

_Comment by @wence- on 2025-05-07 14:42_

> Thanks for the report! Yes, this is a known issue, we're considering possible solutions.

Thanks, happy for this one to be closed if it's captured by other issues.

---

_Renamed from "[ty] panics when type-checking CRTP generic classes" to "panics when type-checking CRTP generic classes" by @MichaReiser on 2025-05-07 15:24_

---

_Label `bug` added by @AlexWaygood on 2025-05-07 17:07_

---

_Comment by @sharkdp on 2025-05-08 14:15_

Thank you for reporting this. I will add your examples to #256 to collect all of them in one place.

---

_Closed by @sharkdp on 2025-05-08 14:15_

---
