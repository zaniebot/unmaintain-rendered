```yaml
number: 1181
title: enforce operator functions include the possible right hand version in their input type
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-14T01:25:58Z
updated_at: 2025-09-16T04:36:25Z
url: https://github.com/astral-sh/ty/issues/1181
synced_at: 2026-01-10T02:06:25Z
```

# enforce operator functions include the possible right hand version in their input type

---

_Issue opened by @KotlinIsland on 2025-09-14 01:25_

### Summary

```py
class Left:
    def __add__(self, other: int) -> int:
        # here "other" is Right
        return other + 1  # TypeError here


class Right:
    def __radd__(self, other: Left) -> str:
        return "i'm string :)"


Left() + Right()
```

here the type of  `Left.__add__`'s `other`, should should be `int | { __radd__: (other: Left) -> object }`, otherwise we should see an error on the last line

# full form
in combination with some of my other issues
```py
class Left:
    @overload
    def __add__(self, other: int) -> int: ...
    @overload
    def __add__(self, other: object) -> NotImplemented: ...

    def __add__(self, other): ...
        # here "other" is object
        if not isinstance(other, int):
            return NotImplemented
        return other + 1  # all good :)


class Right:
    def __radd__(self, other: Left) -> str:
        return "i'm string :)"


Left() + Right()
```

---

_Comment by @carljm on 2025-09-15 19:45_

Yes, the convention that dunder methods should be annotated with the argument types that will cause them to not return `NotImplemented` is unsound, in the sense that it requires the author of the method's internals to handle the unsupported types (and return `NotImplemented` for them) without any help from the type-checker to validate that they have done so. That's a known issue, but nonetheless that's the status-quo expectation in the ecosystem.

I don't see this issue as independently actionable from https://github.com/astral-sh/ty/issues/1150 -- they are both asking for the same change to the status-quo expectation of how dunder methods are annotated, to handle `NotImplemented` possibility explicitly rather than implicitly.

---

_Closed by @carljm on 2025-09-15 19:45_

---

_Comment by @KotlinIsland on 2025-09-16 04:36_

@carljm this issue is demonstrating how the current semantics allow incompatible types to be passed to operator methods

i understand what you mean about it being a part of the larger problem, I will update the other issue with these details and example

---
