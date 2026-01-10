---
number: 2070
title: can’t create a type variable with a default on Python < 3.13 (false positive for invalid-legacy-type-variable)
type: issue
state: open
author: flying-sheep
labels:
  - bug
assignees: []
created_at: 2025-12-18T13:20:37Z
updated_at: 2025-12-23T05:13:06Z
url: https://github.com/astral-sh/ty/issues/2070
synced_at: 2026-01-10T01:52:52Z
---

# can’t create a type variable with a default on Python < 3.13 (false positive for invalid-legacy-type-variable)

---

_Issue opened by @flying-sheep on 2025-12-18 13:20_

### Summary

https://play.ty.dev/1f971569-26a4-4e0b-bfd4-158f9d46fa7f

```python
if TYPE_CHECKING or sys.version_info > (3, 13):
    T = TypeVar("T", default=int)
else:  # runtime on Python < 3.13
    T = TypeVar("T")
```

outputs

> The `default` parameter of `typing.TypeVar` was added in Python 3.13 (invalid-legacy-type-variable) [Ln 5, Col 22]

yeah I know, that’s why I only specify it in valid contexts.

### Version

0.0.3

---

_Renamed from "can’t create a type variable with a default on Python < 3.13" to "can’t create a type variable with a default on Python < 3.13 (false positive for invalid-legacy-type-variable)" by @flying-sheep on 2025-12-18 13:22_

---

_Comment by @AlexWaygood on 2025-12-18 13:22_

Thanks! Definitely a bug. We need to suppress this error when we can see we're in unreachable code blocks

---

_Label `bug` added by @AlexWaygood on 2025-12-18 13:22_

---

_Label `unreachable-code` added by @AlexWaygood on 2025-12-18 13:22_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-18 13:22_

---

_Comment by @sinon on 2025-12-18 13:34_

Is this a bug? If I change the `or` to an `and` (or drop TYPE_CHECKING completely) ty detects the correct branch is `Never` then updating python version in `ty.json` to 3.14 the `Never` branches switch places and `ty` is still happy. 🤔 

https://play.ty.dev/e1f55066-d80f-436a-b7dd-5336c6951107

---

_Comment by @AlexWaygood on 2025-12-18 13:42_

Ah, good point @sinon.

Hmm, yes, it looks like we've correctly deduced that the first branch is always reachable, because you've used an `or` rather than an `and`, @flying-sheep, and `TYPE_CHECKING` is treated as always-true by type checkers. Because the first branch is always reachable, we'll therefore emit the diagnostic there. You can make ty happy by doing something like this:

```py
import sys
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    # type checkers think typing_extensions is part of the stdlib,
    # so it's fine to import in an `if TYPE_CHECKING` block
    # even if it's not a dependency
    from typing_extensions import TypeVar

    T = TypeVar("T", default=int)
elif sys.version_info >= (3, 13):
    from typing import TypeVar

    T = TypeVar("T", default=int)
else:
    from typing import TypeVar

    T = TypeVar("T")
```

But that's admittedly a bit verbose. We could consider just switching this specific rule off if we see you're in an `if TYPE_CHECKING` block?

---

_Label `bug` removed by @AlexWaygood on 2025-12-18 13:48_

---

_Label `unreachable-code` removed by @AlexWaygood on 2025-12-18 13:48_

---

_Comment by @flying-sheep on 2025-12-19 08:23_

@sinon, I updated the playground link to show why your suggestion doesn’t work: https://play.ty.dev/1f971569-26a4-4e0b-bfd4-158f9d46fa7f

I of course want to actually *use* the TypeVar’s default, which wouldn’t exist anymore if you change the logic to `and`:

> Type `int` does not match asserted type `Unknown` (type-assertion-failure) [Ln 12, Col 1]

The `else` branch is there to be executed at runtime and needs to be be ignored at type checking time.

----

> We could consider just switching this specific rule off if we see you're in an if `TYPE_CHECKING` block?

@AlexWaygood yeah, that would work! If type checkers treat `typing_extensions` as special in a type checking block, they can also treat later feature addition to typing APIs (like `default`) in a special way.

---

_Comment by @carljm on 2025-12-23 00:02_

Yeah, we should just turn off this error in `if TYPE_CHECKING` -- I think we've run into other contexts where we want to treat `if TYPE_CHECKING` more like stub code, that we know will never be executed. (Ellipsis function bodies maybe?)

---

_Label `bug` added by @dhruvmanila on 2025-12-23 05:13_

---
