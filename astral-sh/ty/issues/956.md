```yaml
number: 956
title: "Consider storing a `Box<[T]>` or shrink the tuple's inner `Vec'`s before wrapping them as `Type`"
type: issue
state: closed
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-08-08T11:28:08Z
updated_at: 2025-08-12T11:51:17Z
url: https://github.com/astral-sh/ty/issues/956
synced_at: 2026-01-10T02:06:24Z
```

# Consider storing a `Box<[T]>` or shrink the tuple's inner `Vec'`s before wrapping them as `Type`

---

_Issue opened by @MichaReiser on 2025-08-08 11:28_

Both `FixedLengthTupleType` and `VariableLengthTupleType` contain `Vec<T>` fields that we don't shrink before constructing a `TupleTypeSpec` (to my knowledge). This can result in increased memory usage because of the unused elements in those vecs.

https://github.com/astral-sh/ruff/blob/41207ec901340423d988b85dc8138e306f9a39c2/crates/ty_python_semantic/src/types/tuple.rs#L323

https://github.com/astral-sh/ruff/blob/41207ec901340423d988b85dc8138e306f9a39c2/crates/ty_python_semantic/src/types/tuple.rs#L540-L544

---

_Label `memory` added by @MichaReiser on 2025-08-08 11:28_

---

_Closed by @AlexWaygood on 2025-08-12 11:51_

---
