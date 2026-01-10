---
number: 2190
title: support generic TypedDict
type: issue
state: open
author: RyanSaxe
labels:
  - generics
  - typeddict
assignees: []
created_at: 2025-12-23T18:26:39Z
updated_at: 2025-12-23T21:23:24Z
url: https://github.com/astral-sh/ty/issues/2190
synced_at: 2026-01-10T01:52:52Z
---

# support generic TypedDict

---

_Issue opened by @RyanSaxe on 2025-12-23 18:26_

### Summary

TypeVars do not seem to be properly handled inside TypedDicts

```python
from typing import TypedDict, reveal_type


class TestDictExample[T](TypedDict):
    id: int
    name: str
    active: bool
    bug: T


class Example[T]:
    def __init__(self, data: TestDictExample[T]) -> None:
        self.attr: TestDictExample[T] = data

    def do_something(self) -> T:
        value = self.attr["bug"] # ty thinks this is Unknown instead of T
        name = self.attr["name"] # ty correctly gets this as str
        return value
```

https://play.ty.dev/fa58789a-4d72-4f76-802d-7dafe74e52b8

### Version

ty 0.0.5

---

_Renamed from "TypedDict with TypeVars are always treated as Unknown" to "support generic TypedDict" by @carljm on 2025-12-23 19:31_

---

_Comment by @carljm on 2025-12-23 19:31_

Thanks for the report!

---

_Comment by @AlexWaygood on 2025-12-23 20:20_

Huh, I'm pretty sure we have _some_ support for generic TypedDicts. We definitely have a bunch of tests that use generic TypedDicts

---

_Comment by @carljm on 2025-12-23 20:27_

It looks like what we have is pretty limited -- we appear to keep track of the specialization and use it when validating a dict literal against the TypedDict, but we don't display it as part of the type display, or take it into account when inferring a type from `__getitem__`.

We could re-word the title here, but based on what I'm seeing I think the current title is fair :)

---

_Label `generics` added by @AlexWaygood on 2025-12-23 21:23_

---

_Label `typeddict` added by @AlexWaygood on 2025-12-23 21:23_

---
