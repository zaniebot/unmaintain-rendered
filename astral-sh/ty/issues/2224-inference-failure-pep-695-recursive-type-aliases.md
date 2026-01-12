```yaml
number: 2224
title: "Inference failure: PEP 695 recursive type aliases resolve to 'Unknown'"
type: issue
state: closed
author: Blinkion
labels: []
assignees: []
created_at: 2025-12-26T02:51:19Z
updated_at: 2025-12-26T18:36:46Z
url: https://github.com/astral-sh/ty/issues/2224
synced_at: 2026-01-12T15:54:26Z
```

# Inference failure: PEP 695 recursive type aliases resolve to 'Unknown'

---

_@Blinkion_

Summary
ty (v0.0.7) currently fails to resolve recursive type aliases defined using the PEP 695 type statement (introduced in Python 3.12). While the syntax is parsed without errors, the inference engine cannot resolve the recursive structure, leading to Unknown types in Inlay Hints and breaking downstream type safety.

Reproduction

```python
from __future__ import annotations

# Standard PEP 695 recursive type alias
type JSONValue = (
    dict[str, JSONValue] | list[JSONValue] | str | int | float | bool | None
)

# Observed inference failure
test_data = {"a": {"b": 1}}
```

Actual Behavior
The Inlay Hint for test_data displays: dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]

It seems the inference engine treats the recursive reference JSONValue within the definition as an unknown or unresolved type during the mapping phase.

Expected Behavior
ty should correctly resolve the recursive alias. The Inlay Hint should ideally show JSONValue or its concrete expansion without any Unknown segments.

Environment
ty version: 0.0.7 (cf82a04b5 2025-12-24)

Python version: 3.14.2 (using PEP 695 syntax)

OS: macOS

Additional Context
Using the older JSONValue: TypeAlias = ... or string forward references results in similar Unknown inference or unresolved-reference errors.

This breakdown of type inference renders the static analysis ineffective for deeply nested data structures (like JSON).

<img width="854" height="296" alt="Image" src="https://github.com/user-attachments/assets/df16ef74-1d0e-4ac6-bdab-b6122a424b3c" />

---

_Renamed from "Inference failure for recursive types (JSONValue) using string forward references" to "Inference failure: PEP 695 recursive type aliases resolve to 'Unknown'" by @Blinkion on 2025-12-26 03:06_

---

_Label `bidirectional inference` added by @mtshiba on 2025-12-26 03:39_

---

_Comment by @carljm on 2025-12-26 17:40_

In your code sample, the type alias `JSONValue` is unused. The inference you see for the type of `test_data` is not related to a recursive type alias at all, it's just our normal inference for un-annotated invariant container literals -- the `Unknown` are to avoid false positives if you didn't intend this dict to be limited to string keys and dictionary values. See #1240 where we plan to provide an option to disable this behavior.

If you change your code example as follows, to use the `JSONValue` type:

```py
from __future__ import annotations

type JSONValue = (
    dict[str, JSONValue] | list[JSONValue] | str | int | float | bool | None
)

test_data: JSONValue = {"a": {"b": 1}}
reveal_type(test_data) 
```

this reveals a real issue, where we choose to still infer `test_data` as the "narrower" inferred dictionary type, rather than as `JSONValue`. This will be fixed by #136.

We do understand the recursive nature of a PEP 695 recursive type alias, as shown in this example:

```py
from __future__ import annotations

type JSONValue = (
    dict[str, JSONValue] | list[JSONValue] | str | int | float | bool | None
)

def f(jsonval: JSONValue):
    reveal_type(jsonval)
    if isinstance(jsonval, dict):
        reveal_type(jsonval)
        reveal_type(jsonval["foo"])
```

Though at a certain point we do currently still fall back to `Any` at the point of recursion -- this is a known limitation tracked in #1157.

---

_Closed by @carljm on 2025-12-26 17:40_

---

_Label `bidirectional inference` removed by @AlexWaygood on 2025-12-26 18:36_

---
