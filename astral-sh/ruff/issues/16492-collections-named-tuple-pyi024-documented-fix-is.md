```yaml
number: 16492
title: "collections-named-tuple (PYI024): Documented fix is not equivalent"
type: issue
state: closed
author: Avasam
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-03-04T09:05:03Z
updated_at: 2025-06-21T01:49:19Z
url: https://github.com/astral-sh/ruff/issues/16492
synced_at: 2026-01-10T11:09:57Z
```

# collections-named-tuple (PYI024): Documented fix is not equivalent

---

_Issue opened by @Avasam on 2025-03-04 09:05_

### Summary

From the doc:

[Example](https://docs.astral.sh/ruff/rules/collections-named-tuple/#example)
```py
from collections import namedtuple

person = namedtuple("Person", ["name", "age"])
```
Use instead:
```py
from typing import NamedTuple

class Person(NamedTuple):
    name: str
    age: int
```

But that's not equivalent! Since the variable name `person` differs from the class name `Person`.

For this to be equivalent:
```py
from collections import namedtuple

person = namedtuple("Person", ["name", "age"])
Rect = namedtuple("Rect", ["x", "y"])
```
This would be used:
```py
from typing import NamedTuple

class person(NamedTuple):
    __name__ = "Person"
    name: str
    age: int

class Rect(NamedTuple):
    x: int
    y: int
```
or
```py
from typing import NamedTuple

class Person(NamedTuple):
    name: str
    age: int
person = Person  # old name

class Rect(NamedTuple):
    x: int
    y: int
```

I wouldn't recommend that usage. But backwards compatibility is important in libraries, where NamedTuples matter more.

---

_Comment by @MichaReiser on 2025-03-04 09:22_

It took me a while to understand what the difference is because I thought it was some subtlety with named tuples when all it is is just the naming of the `person` variable.

I suggest that we simply change the example in the docs to:

```py
from collections import namedtuple

Person = namedtuple("Person", ["name", "age"])
```

The rule doesn't have an auto fix and library authors should be aware that changing the casing of an exported variable (if it is exported) is a breaking change.


---

_Label `documentation` added by @MichaReiser on 2025-03-04 09:22_

---

_Label `help wanted` added by @MichaReiser on 2025-03-04 09:22_

---

_Comment by @Avasam on 2025-03-04 09:39_

(I'm going to bed but doc changes are easy, no need for rust knowledge and I can do it tomorrow if no one completed it already, I just needed a decision)

---

_Comment by @MichaReiser on 2025-03-04 09:43_

Thank you and thanks for reporting the issue. We could consider adding a snapshot test for this with a comment. Just in case we ever add a fix for this (as you requested in the other issue). This seems easy to overlook

---

_Closed by @AlexWaygood on 2025-06-20 13:30_

---

_Comment by @Avasam on 2025-06-21 01:49_

I completely forgot about this. Thanks!

---
