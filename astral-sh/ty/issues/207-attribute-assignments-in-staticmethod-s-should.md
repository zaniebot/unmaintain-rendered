```yaml
number: 207
title: "Attribute assignments in `@staticmethod`s should not influence instance attributes"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - runtime semantics
  - attribute access
assignees: []
created_at: 2025-02-05T11:45:09Z
updated_at: 2025-06-27T14:25:44Z
url: https://github.com/astral-sh/ty/issues/207
synced_at: 2026-01-12T15:54:22Z
```

# Attribute assignments in `@staticmethod`s should not influence instance attributes

---

_@sharkdp_

The goal of this issue is to get rid of the `TODO` comments in [this test](https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/red_knot_python_semantic/resources/mdtest/attributes.md#static-methods-do-not-influence-implicitly-defined-attributes). An attribute assignment in a `@staticmethod` should not influence the existence of implicit (instance) attributes:

```py
class Unrelated:
    x: int

class C:
    @staticmethod
    def f(unrelated: Unrelated):
        unrelated.x = 1

reveal_type(C().x)  # should be an error
```

Whether or not this requires special-casing of `@staticmethod` during semantic index building is an open question. The answer to this question influences whether or not we can also handle the more exotic edge-cases in the mentioned test (e.g. aliased `@staticmethod`).

Part of: astral-sh/ruff#14164 

---

_Renamed from "Make sure that attribute assignments in `@staticmethod`s are not considered to be instance attributes" to "[red-knot] Attribute assignments in `@staticmethod`s should not influence instance attributes" by @sharkdp on 2025-02-05 11:45_

---

_Label `bug` added by @sharkdp on 2025-02-05 11:47_

---

_Renamed from "[red-knot] Attribute assignments in `@staticmethod`s should not influence instance attributes" to "Attribute assignments in `@staticmethod`s should not influence instance attributes" by @MichaReiser on 2025-05-07 15:26_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:24_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:05_

---

_Comment by @carljm on 2025-06-27 14:25_

Per the description of this issue, this was fixed in https://github.com/astral-sh/ruff/pull/18809 -- the TODOs in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/attributes.md#static-methods-do-not-influence-implicitly-defined-attributes are gone.

---

_Closed by @carljm on 2025-06-27 14:25_

---
