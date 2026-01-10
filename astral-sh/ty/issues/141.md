```yaml
number: 141
title: "Fix `@no_type_check` regression involving unknown decorators"
type: issue
state: open
author: sharkdp
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-04-02T07:36:35Z
updated_at: 2025-12-17T14:07:56Z
url: https://github.com/astral-sh/ty/issues/141
synced_at: 2026-01-10T01:54:59Z
```

# Fix `@no_type_check` regression involving unknown decorators

---

_Issue opened by @sharkdp on 2025-04-02 07:36_

astral-sh/ruff#17017 introduced a regression in our understanding of `@no_type_check`, leading to two open `TODO`s in the tests here:

https://github.com/astral-sh/ruff/blob/ae2cf91a360e927f5da307d0290be1a6229ced9a/crates/red_knot_python_semantic/resources/mdtest/suppressions/no_type_check.md?plain=1#L41-L72

Addressing this might involve introducing a new function (salsa query?) that explicitly checks for the existence of a `@no_type_check` decorator in the definition of the function that is currently being type checked.

---

_Label `bug` added by @sharkdp on 2025-04-02 07:36_

---

_Renamed from "[red-knot] Fix `@no_type_check` regression involving unknown decorators" to "Fix `@no_type_check` regression involving unknown decorators" by @MichaReiser on 2025-05-07 15:25_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:54_

---

_Comment by @NathanCQC on 2025-12-17 14:07_

I am having some problems combining `@no_type_check` with the `@guppy` decorator from [guppylang](https://github.com/Quantinuum/guppylang/tree/main), when using ty.

When both are combined, it does not disable type checking within the `@guppy` decorated function.

---
