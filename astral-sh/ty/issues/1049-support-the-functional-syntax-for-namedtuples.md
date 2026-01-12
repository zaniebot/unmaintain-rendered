```yaml
number: 1049
title: Support the functional syntax for NamedTuples
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
  - namedtuples
assignees: []
created_at: 2025-08-19T11:34:20Z
updated_at: 2025-12-31T20:09:34Z
url: https://github.com/astral-sh/ty/issues/1049
synced_at: 2026-01-12T15:54:24Z
```

# Support the functional syntax for NamedTuples

---

_@AlexWaygood_

Ty now supports most features of `NamedTuple`s defined via the class-based syntax (https://github.com/astral-sh/ty/issues/545). However, we do not yet have full support for namedtuples defined using the function-call syntax. I suggest that we hold off on this until work on https://github.com/astral-sh/ty/issues/742 is complete, as some refactoring that needs to be done for that issue will likely be helpful for this issue.

We should add suppport for these ways of defining a namedtuple:

```py
from collections import namedtuple
from typing import NamedTuple

a = namedtuple("a", "a b c")
b = namedtuple("b", "a, b, c")
c = namedtuple("c", ["a", "b", "c"])
d = namedtuple("d", ("a", "b", "c"))

e = NamedTuple("e", [("a", int), ("b", str)])
f = NamedTuple("f", (("a", int), ("b", str)))
```

The first four should all be inferred as being `NamedTuple` classes that inherit from `tuple[Unknown, Unknown, Unknown]`; the last two should both be inferred as being `NamedTuple` classes that inherit from `tuple[int, str]`.

There are some additional rules and restrictions about the functional syntax that are described at the bottom of https://typing.python.org/en/latest/spec/namedtuples.html#defining-named-tuples and at https://docs.python.org/3/library/collections.html#collections.namedtuple

---

_Added to milestone `GA` by @AlexWaygood on 2025-08-19 11:34_

---

_Label `typing semantics` added by @AlexWaygood on 2025-08-19 11:34_

---

_Label `namedtuples` added by @AlexWaygood on 2025-11-29 18:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-31 20:09_

---
