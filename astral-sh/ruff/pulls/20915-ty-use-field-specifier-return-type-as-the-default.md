```yaml
number: 20915
title: "[ty] Use field-specifier return type as the default type for the field"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dataclass-field-default-type
created_at: 2025-10-16T11:00:38Z
updated_at: 2025-10-16T11:13:47Z
url: https://github.com/astral-sh/ruff/pull/20915
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Use field-specifier return type as the default type for the field

---

_Pull request opened by @sharkdp on 2025-10-16 11:00_

## Summary

`dataclasses.field` and field-specifier functions of commonly used libraries like `pydantic`, `attrs`, and `SQLAlchemy` all return the default type for the field (or `Any`) instead of an actual `Field` instance, even if this is not what happens at runtime. Let's make use of this fact and assume that *all* field specifiers return the type of the default value of the field.

For standard dataclasses, this leads to more or less the same outcome (see test diff for details), but this change is important for 3rd party dataclass-transformers.

## Test Plan

Tested the consequences of this change on the field-specifiers branch as well.

---

_Review requested from @carljm by @sharkdp on 2025-10-16 11:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-16 11:00_

---

_Review requested from @dcreager by @sharkdp on 2025-10-16 11:00_

---

_Label `ty` added by @sharkdp on 2025-10-16 11:00_

---

_@sharkdp reviewed on 2025-10-16 11:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:14 on 2025-10-16 11:03_

This happens due to literal promotion, I guess.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:40 on 2025-10-16 11:03_

This shows that our previous approach, calling `default_factory` manually, was apparently too naive.

---

_@sharkdp reviewed on 2025-10-16 11:03_

---

_Comment by @github-actions[bot] on 2025-10-16 11:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 11:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-10-16 11:12_

---

_Merged by @sharkdp on 2025-10-16 11:13_

---

_Closed by @sharkdp on 2025-10-16 11:13_

---

_Branch deleted on 2025-10-16 11:13_

---
