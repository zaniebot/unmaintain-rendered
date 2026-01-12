```yaml
number: 727
title: "`invalid-type-form` emitted for non-type tuple index expressions"
type: issue
state: open
author: sharkdp
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-06-30T07:59:27Z
updated_at: 2025-11-18T16:10:34Z
url: https://github.com/astral-sh/ty/issues/727
synced_at: 2026-01-12T15:54:23Z
```

# `invalid-type-form` emitted for non-type tuple index expressions

---

_@sharkdp_

ty emits `invalid-type-form` for non-type expressions like the following that do not produce errors at runtime:
```py
tuple[1]
```

While the above case may seem nonsensical, there are real-world code examples where a strict treatment of non-type `tuple[ ]` expressions under the restricted grammar of type-expressions would lead to false positives (https://github.com/astral-sh/ruff/pull/18991#issuecomment-3018171342). For example:
```py
runtime_type_representation = tuple[(int, str) * 3]
```

---

_Label `bug` added by @sharkdp on 2025-06-30 07:59_

---

_Label `type-inference` added by @sharkdp on 2025-06-30 07:59_

---

_Comment by @AlexWaygood on 2025-06-30 13:32_

One way to fix this might be to:
1. Refactor `infer_type_expression()` to return a `Result` rather than eagerly emitting diagnostics (this might also help with https://github.com/astral-sh/ty/issues/667)
2. In non-annotation contexts, attempt to parse the inner value as a type expression (and infer a `Type::GenericAlias` type if so, like we do on `main`), but if that returns an `Err`, fall back to `Type::NominalInstance("GenericAlias")` without emitting a diagnostic

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:28_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
