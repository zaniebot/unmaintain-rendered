```yaml
number: 297
title: Panic on Type-Checking Recursive JSON
type: issue
state: closed
author: max-muoto
labels: []
assignees: []
created_at: 2025-05-09T16:34:35Z
updated_at: 2025-05-09T16:37:35Z
url: https://github.com/astral-sh/ty/issues/297
synced_at: 2026-01-12T15:54:22Z
```

# Panic on Type-Checking Recursive JSON

---

_@max-muoto_

### Summary

A pretty simple example of a recursive type that's supported in other type-checkers such as Pyright is a JSON type:

```python
from __future__ import annotations

type Json = dict[str, Json] | list[Json] | str | int | float | bool | None


def parse_json(j: Json) -> Json:
    if isinstance(j, dict):
        return {k: parse_json(v) for k, v in j.items()}
    elif isinstance(j, list):
        return [parse_json(i) for i in j]
    else:
        return


print(parse_json({"a": {"b": "c"}}))
```

This example yields a panic:

```
error: panic: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/f78a641/src/function/fetch.rs:131:25 when checking `/Users/maxmuoto/projects/ty-test/main.py`: `dependency graph cycle when querying value_type_(Id(3400)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(800)),
    infer_scope_types(Id(c00)),
    signature_(Id(3800)),
    infer_deferred_types(Id(1006)),
    value_type_(Id(3400)),
    infer_scope_types(Id(c01)),
    symbol_by_id(Id(2c17)),
    use_def_map(Id(c00)),
    infer_definition_types(Id(134d)),
    infer_expression_types(Id(1434)),
    infer_definition_types(Id(4bc0)),
    infer_expression_types(Id(1499)),
    infer_definition_types(Id(4bc3)),
    infer_expression_types(Id(149c)),
    source_text(Id(202e)),
    infer_deferred_types(Id(450c)),
    infer_definition_types(Id(133a)),
    member_lookup_with_policy_(Id(2809)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["ty", "check", "main.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(c01))
             at crates/ty_python_semantic/src/types/infer.rs:120
   1: value_type_(Id(3400))
             at crates/ty_python_semantic/src/types.rs:7384
   2: infer_deferred_types(Id(1006))
             at crates/ty_python_semantic/src/types/infer.rs:185
   3: signature_(Id(3800))
             at crates/ty_python_semantic/src/types.rs:6622
   4: infer_scope_types(Id(c00))
             at crates/ty_python_semantic/src/types/infer.rs:120
   5: check_types(Id(800))
             at crates/ty_python_semantic/src/types.rs:83
```


### Version

ty 0.0.0-alpha.7 (905a3e1e5 2025-05-07)

---

_Closed by @AlexWaygood on 2025-05-09 16:35_

---

_Comment by @max-muoto on 2025-05-09 16:36_

Apologies for the duplicate - missed that!

---

_Comment by @AlexWaygood on 2025-05-09 16:37_

no problem!

---
