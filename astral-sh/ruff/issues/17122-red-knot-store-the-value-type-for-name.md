```yaml
number: 17122
title: "[red-knot] Store the value type for name expressions in store context."
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2025-04-01T14:42:36Z
updated_at: 2025-05-01T18:33:52Z
url: https://github.com/astral-sh/ruff/issues/17122
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Store the value type for name expressions in store context.

---

_Issue opened by @MichaReiser on 2025-04-01 14:42_

Given:

```py
class Name:
    ...

class Foo:
    name: Name
```

Red Knot stores `Type::Never` for the `ExprName` node declaring the attribute `name`. In pseudo code:

```
class Name:
    ...

class Foo:
    reveal_type(name): Name
```

Storing `Type::Never` has the downside that goto type definition and other LSP operations don't work as expected (unless we handle these expressions differently). 

We should consider storing the *new* value type for the `ExprName` node instead.

---

_Label `red-knot` added by @MichaReiser on 2025-04-01 14:42_

---

_Added to milestone `Red Knot Beta` by @MichaReiser on 2025-04-01 14:42_

---

_Removed from milestone `Red Knot Beta` by @carljm on 2025-04-07 20:19_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-07 20:19_

---

_Assigned to @sharkdp by @sharkdp on 2025-04-28 11:35_

---

_Closed by @sharkdp on 2025-05-01 18:33_

---
