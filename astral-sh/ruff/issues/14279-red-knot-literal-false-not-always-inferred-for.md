```yaml
number: 14279
title: "[red-knot] `Literal[False]` not always inferred for comparisons between tuple types with disjunct literal elements"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
  - ty
assignees: []
created_at: 2024-11-11T13:41:12Z
updated_at: 2024-11-20T01:32:44Z
url: https://github.com/astral-sh/ruff/issues/14279
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] `Literal[False]` not always inferred for comparisons between tuple types with disjunct literal elements

---

_Issue opened by @AlexWaygood on 2024-11-11 13:41_

red-knot infers the following types for the following comparisons:

```py
from typing import reveal_type

def int_instance() -> int:
    return 42

reveal_type("foo" == "bar")                                       # Literal[False]
reveal_type(("foo",) == ("bar",))                                 # Literal[False]
reveal_type((4, "foo") == (4, "bar"))                             # Literal[False]
reveal_type((int_instance(), "foo") == (int_instance(), "bar"))   # bool
```

The last one should also be inferred as `Literal[False]`, just like the first three. This is because two objects of types `Literal["foo"]` and `Literal["bar"]` respectively will never compare equal to each other. By the same token, two 2-element tuples where the second elements are of type `Literal["foo"]` and `Literal["bar"]` respectively will never compare equal, regardless of what their first element is.

I discovered this while working on https://github.com/astral-sh/ruff/pull/14244. Ideally, this comparison would be inferred as `Literal[False]`, since `sys.version_info[3]` should be inferred by red-knot as being of type `Literal["alpha", "beta", "candidate", "final"]`, and objects with that type never compare equal to the string `"finallllllll"`:

```py
import sys
from typing import reveal_type

reveal_type(sys.version_info == (3, 8, 1, "finallllllll", 0))
```

---

_Label `bug` added by @AlexWaygood on 2024-11-11 13:41_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-11 13:41_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-11 13:41_

---

_Comment by @cake-monotone on 2024-11-12 05:18_

That makes sense! Additionally, we should handle tuples of different lengths.

```py
reveal_type((int_instance(),) == (int_instance(), "bar"))   # revealed: bool
```

I think it might be a simple fix! If no other opinions come up, I’ll go ahead and work on it :)

---

_Comment by @cake-monotone on 2024-11-12 06:23_

An issue with error propagation suddenly came to mind. When tuple comparison was initially implemented, only literal comparison was supported, so there was no opportunity to properly test error propagation.

```py
(int_instance(), "test") < (int_instance(), 3)
```

Since there’s a possibility of an unsupported operation error here, a diagnostic should be triggered. However, it currently isn’t, as the comparison stops immediately when `Truthness::Ambiguous` occurs during lexicographic comparison.

---

_Comment by @AlexWaygood on 2024-11-12 07:44_

Yes, I think you're correct on both counts!

---

_Closed by @carljm on 2024-11-20 01:32_

---

_Closed by @carljm on 2024-11-20 01:32_

---
