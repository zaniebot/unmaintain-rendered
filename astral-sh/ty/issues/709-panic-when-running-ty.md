```yaml
number: 709
title: panic when running ty
type: issue
state: closed
author: Matt-Ord
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-06-26T09:17:25Z
updated_at: 2025-07-03T08:48:36Z
url: https://github.com/astral-sh/ty/issues/709
synced_at: 2026-01-12T15:54:23Z
```

# panic when running ty

---

_@Matt-Ord_

### Summary

```
slate-corevscode ➜ /workspaces/slate (annotations) $ ty check ./slate_core/plot/
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/09627e4/src/function/fetch.rs:165:25 when checking `/workspaces/slate/slate_core/plot/_plot.py`: `dependency graph cycle when querying value_type_(Id(2a800)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(c05)),
    infer_scope_types(Id(8000)),
    infer_deferred_types(Id(8498)),
    explicit_bases_(Id(18005)),
    infer_scope_types(Id(4d37)),
    cached_protocol_interface(Id(c80a)),
    infer_definition_types(Id(11736)),
    signature_(Id(16424)),
    infer_deferred_types(Id(11736)),
    value_type_(Id(2a800)),
    infer_scope_types(Id(4c95)),
    place_by_id(Id(fc3f)),
    place_by_id(Id(fc3d)),
    infer_definition_types(Id(5215)),
    try_mro_(Id(28009)),
    infer_expression_types(Id(2dfa)),
    infer_definition_types(Id(20d74)),
    infer_expression_types(Id(2df3)),
    explicit_bases_(Id(18013)),
    member_lookup_with_policy_(Id(b03a)),
    place_by_id(Id(fc25)),
    infer_definition_types(Id(1d4a4)),
    member_lookup_with_policy_(Id(b03b)),
    place_by_id(Id(fc26)),
    infer_definition_types(Id(1cf7e)),
    infer_definition_types(Id(1cf7c)),
    member_lookup_with_policy_(Id(b042)),
    place_by_id(Id(fc30)),
    try_mro_(Id(28002)),
    member_lookup_with_policy_(Id(b040)),
    place_by_id(Id(fc2b)),
    infer_definition_types(Id(19d15)),
    member_lookup_with_policy_(Id(b041)),
    place_by_id(Id(fc2c)),
    infer_definition_types(Id(20bd)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "./slate_core/plot/"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(4c95))
             at crates/ty_python_semantic/src/types/infer.rs:139
   1: value_type_(Id(2a800))
             at crates/ty_python_semantic/src/types.rs:7420
   2: infer_deferred_types(Id(11736))
             at crates/ty_python_semantic/src/types/infer.rs:207
   3: signature_(Id(16424))
             at crates/ty_python_semantic/src/types/function.rs:526
             cycle heads: infer_definition_types(Id(11736)) -> IterationCount(1)
   4: infer_definition_types(Id(11736))
             at crates/ty_python_semantic/src/types/infer.rs:168
   5: cached_protocol_interface(Id(c80a))
             at crates/ty_python_semantic/src/types/protocol_class.rs:330
   6: infer_scope_types(Id(4d37))
             at crates/ty_python_semantic/src/types/infer.rs:139
   7: explicit_bases_(Id(18005))
             at crates/ty_python_semantic/src/types/class.rs:763
   8: infer_deferred_types(Id(8498))
             at crates/ty_python_semantic/src/types/infer.rs:207
   9: infer_scope_types(Id(8000))
             at crates/ty_python_semantic/src/types/infer.rs:139
  10: check_types(Id(c05))
             at crates/ty_python_semantic/src/types.rs:89


error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/09627e4/src/function/fetch.rs:165:25 when checking `/workspaces/slate/slate_core/plot/_animate.py`: `dependency graph cycle when querying value_type_(Id(2a800)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_types(Id(c02)),
    infer_scope_types(Id(4c04)),
    infer_definition_types(Id(5029)),
    infer_expression_types(Id(5c05)),
    signature_(Id(12001)),
    infer_deferred_types(Id(521b)),
    value_type_(Id(2a800)),
    infer_scope_types(Id(4c95)),
    infer_definition_types(Id(217f)),
    infer_definition_types(Id(1c498)),
    member_lookup_with_policy_(Id(a028)),
    place_by_id(Id(ec1c)),
    place_by_id(Id(ec1a)),
    infer_definition_types(Id(218b)),
    infer_definition_types(Id(203f)),
    member_lookup_with_policy_(Id(a027)),
    place_by_id(Id(ec1b)),
    infer_definition_types(Id(1ae27)),
    file_to_module(Id(11022)),
    source_text(Id(11022)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "./slate_core/plot/"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(4c95))
             at crates/ty_python_semantic/src/types/infer.rs:139
   1: value_type_(Id(2a800))
             at crates/ty_python_semantic/src/types.rs:7420
   2: infer_deferred_types(Id(521b))
             at crates/ty_python_semantic/src/types/infer.rs:207
   3: signature_(Id(12001))
             at crates/ty_python_semantic/src/types/function.rs:526
   4: infer_expression_types(Id(5c05))
             at crates/ty_python_semantic/src/types/infer.rs:244
   5: infer_definition_types(Id(5029))
             at crates/ty_python_semantic/src/types/infer.rs:168
   6: infer_scope_types(Id(4c04))
             at crates/ty_python_semantic/src/types/infer.rs:139
   7: check_types(Id(c02))
             at crates/ty_python_semantic/src/types.rs:89

```

full code can be found at
https://github.com/Matt-Ord/slate

### Version

ty 0.0.1-alpha.12

---

_Label `bug` added by @AlexWaygood on 2025-06-26 10:35_

---

_Label `fatal` added by @AlexWaygood on 2025-06-26 10:35_

---

_Comment by @Matt-Ord on 2025-07-03 08:05_

@AlexWaygood just to confirm that this still exists in 13. I narrowed down the root cause to

```
from __future__ import annotations

import numpy as np

type NestedLength = int | tuple[NestedLength, ...]


def size_from_nested_shape(shape: NestedLength) -> int:
    """Get the size from a nested shape."""
    if isinstance(shape, int):
        return shape
    return np.prod([size_from_nested_shape(sub_shape) for sub_shape in shape]).item()

```

Interestingly, ty check hangs if I remove size_from_nested_shape

```
from __future__ import annotations

type NestedLength = int | tuple[NestedLength, ...]
```

---

_Comment by @sharkdp on 2025-07-03 08:47_

> I narrowed down the root cause

Thank you! This looks like another case of https://github.com/astral-sh/ty/issues/256.

> Interestingly, ty check hangs if I remove size_from_nested_shape

Thanks. I'll mention that in a comment on the linked ticket.

A single use of the type turns this into a panic:

```py
from __future__ import annotations

type NestedLength = int | tuple[NestedLength, ...]

n: NestedLength
```

---

_Closed by @sharkdp on 2025-07-03 08:47_

---
