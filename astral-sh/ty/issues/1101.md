```yaml
number: 1101
title: "support `type[...]` where `...` is a generic alias"
type: issue
state: closed
author: carljm
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-08-27T21:05:29Z
updated_at: 2025-11-24T21:56:44Z
url: https://github.com/astral-sh/ty/issues/1101
synced_at: 2026-01-10T01:58:59Z
```

# support `type[...]` where `...` is a generic alias

---

_Issue opened by @carljm on 2025-08-27 21:05_

https://play.ty.dev/1ac81fdc-057b-487a-9910-1d690b4ca673

```py
class C[T]:
    pass

class D[T]:
    pass

var: type[C[int]] = D[int]

```

This assignment should be an error, but currently isn't.

---

_Added to milestone `Beta` by @carljm on 2025-08-27 21:05_

---

_Label `bug` added by @carljm on 2025-08-27 21:05_

---

_Label `type properties` added by @carljm on 2025-08-27 21:05_

---

_Assigned to @dcreager by @carljm on 2025-09-19 14:52_

---

_Comment by @AlexWaygood on 2025-11-10 12:26_

The behaviour here is just because we don't support `type[C[int]]` in user annotations yet -- you're hitting this TODO branch: https://github.com/astral-sh/ruff/blob/f44598dc113a7380ca52d6c969431ca929a93473/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L683-L686

If you workaround the missing piece in our type-expression parsing, you'll find that we emit the error you expect here:

```py
from ty_extensions import TypeOf

class C[T]:
    pass

class D[T]:
    pass

def f(x: C[int]):
    meta_x = type(x)
    reveal_type(meta_x)  # revealed: type[C[int]]
    _: TypeOf[meta_x] = D[int]  # error[invalid-assignment] "Object of type `<class 'D[int]'>` is not assignable to `type[C[int]]`"
```

https://play.ty.dev/eb85e9ee-1ce4-420e-bb61-6bfdf6297985

Possibly fixing the TODO branch in https://github.com/astral-sh/ruff/blob/f44598dc113a7380ca52d6c969431ca929a93473/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L683-L686 should also be a Beta issue, but it's a pretty different issue to the one originally reported here 😄

---

_Unassigned @dcreager by @MichaReiser on 2025-11-10 16:31_

---

_Assigned to @oconnor663 by @MichaReiser on 2025-11-10 16:31_

---

_Renamed from "wrong assignability of mismatched GenericAlias to a SubclassOf type" to "support `type[...]` where `...` is a generic alias" by @carljm on 2025-11-10 16:32_

---

_Closed by @oconnor663 on 2025-11-24 21:56_

---
