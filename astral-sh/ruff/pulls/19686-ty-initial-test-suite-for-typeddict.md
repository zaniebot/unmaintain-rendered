```yaml
number: 19686
title: "[ty] Initial test suite for `TypedDict`"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/typeddict-tests
created_at: 2025-08-01T13:13:17Z
updated_at: 2025-08-01T14:56:04Z
url: https://github.com/astral-sh/ruff/pull/19686
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Initial test suite for `TypedDict`

---

_Pull request opened by @sharkdp on 2025-08-01 13:13_

## Summary

Adds an initial set of tests based on the highest-priority items in https://github.com/astral-sh/ty/issues/154. This is certainly not yet exhaustive (required/non-required, `total`, and other things are missing), but will be useful to measure progress on this feature.

## Test Plan

Checked intended behavior against runtime and other type checkers.

---

_Label `testing` added by @sharkdp on 2025-08-01 13:13_

---

_Review requested from @carljm by @sharkdp on 2025-08-01 13:13_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-01 13:13_

---

_Review requested from @dcreager by @sharkdp on 2025-08-01 13:13_

---

_Label `ty` added by @sharkdp on 2025-08-01 13:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:18 on 2025-08-01 13:58_

This is a bit nitpicky, but I'd prefer for us to be very precise with our language here. At runtime, there's no object `x` for which `isinstance(x, Person)` will return `True` -- in fact, you get an exception raised if you try to do that!

```pycon
>>> import typing
>>> class Person(typing.TypedDict):
...     name: str
...     age: int | None
...     
>>> alice: Person = {"name": "Alice", "age": None}
>>> isinstance(alice, Person)
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    isinstance(alice, Person)
    ~~~~~~~~~~^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 3231, in __subclasscheck__
    raise TypeError('TypedDict does not support instance and class checks')
TypeError: TypedDict does not support instance and class checks
```

So rather than _instances of the class_ here (a runtime concept), I would talk about _inhabitants of the type_ (a static concept).

Also: I would describe it as "accessing keys" rather than "accessing properties":

```suggestion
New inhabitants can be created from dict literals. When accessing keys, the correct types should
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:30 on 2025-08-01 14:00_

```suggestion
Inhabitants can also be created through a constructor call:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:36 on 2025-08-01 14:00_

```suggestion
Methods that are available on `dict` are also available on `TypedDict` inhabitants:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:42 on 2025-08-01 14:01_

```suggestion
Construction of inhabitants is checked for type correctness:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:93 on 2025-08-01 14:03_

but notably (and, often, confusingly): they _cannot_ be assigned to `dict[str, Any]`. (I can't remember the precise reason for this.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:122 on 2025-08-01 14:04_

```suggestion
class itself, nor on inhabitants of the type defined by the class:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:140 on 2025-08-01 14:05_

```suggestion
`TypedDict` class definitions have some special properties that can be used for introspection:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:214 on 2025-08-01 14:06_

could be worth adding a PEP-695 example too?

---

_@AlexWaygood approved on 2025-08-01 14:06_

Nice!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:93 on 2025-08-01 14:20_

I assume it's because `Mapping` is read-only? If they were assignable to `dict[str, Any]`, we could pass them to a function that modifies/deletes keys in ways that would break `TypedDict` invariants?

---

_@sharkdp reviewed on 2025-08-01 14:20_

---

_@AlexWaygood reviewed on 2025-08-01 14:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:93 on 2025-08-01 14:22_

hmm, yes, that sounds about right

---

_@sharkdp reviewed on 2025-08-01 14:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:18 on 2025-08-01 14:25_

Thanks. I don't mind nitpicky comments, quite the contrary. Keep them coming ;-)

---

_Merged by @sharkdp on 2025-08-01 14:56_

---

_Closed by @sharkdp on 2025-08-01 14:56_

---

_Branch deleted on 2025-08-01 14:56_

---
