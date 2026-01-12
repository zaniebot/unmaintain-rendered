```yaml
number: 719
title: Type narrowing fails to account for reassignment in a conditional based on an intermediate variable (aliased conditional expressions)
type: issue
state: open
author: matthewlloyd
labels:
  - narrowing
assignees: []
created_at: 2025-06-27T20:20:36Z
updated_at: 2026-01-08T19:41:33Z
url: https://github.com/astral-sh/ty/issues/719
synced_at: 2026-01-12T15:54:23Z
```

# Type narrowing fails to account for reassignment in a conditional based on an intermediate variable (aliased conditional expressions)

---

_@matthewlloyd_

### Summary

Ty fails to correctly narrow the type of a variable when a `is None` check is stored in an intermediate boolean variable, and the original variable is then reassigned within a conditional block that uses that boolean.

In the example below, `int_or_none` starts as `int | None`. A boolean `is_none` is created to store the result of `int_or_none is None`. Inside an `if is_none:` block, `int_or_none` is reassigned to an `int`. After this conditional block, the type of `int_or_none` is guaranteed to be `int`. However, Ty still considers its type to be `int | None`, leading to a false positive `invalid-argument-type` error.

Other type checkers like Pyright correctly narrow the type in this scenario.

#### Minimal Reproducible Example

```python
import random


def return_int_or_none() -> int | None:
    if random.randint(0, 1):
        return 1
    return None

def take_int_only(must_be_int: int):
    pass

int_or_none: int | None = return_int_or_none()
is_none = int_or_none is None
if is_none:
    int_or_none = 1

# At this point, int_or_none is guaranteed to be an `int`.
# If it was None, it was reassigned to 1.
# If it was an int, it remains an int.
take_int_only(int_or_none)
```

#### Current Behavior (Ty's Output)

Ty incorrectly reports an error, failing to narrow the type of `int_or_none`.

```shell
$ uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `take_int_only` is incorrect
  --> test.py:17:15
   |
15 |     int_or_none = 1
16 | # At this point, int_or_none is guaranteed to be an `int`.
17 | take_int_only(int_or_none)
   |               ^^^^^^^^^^^ Expected `int`, found `int | None`
   |
info: Function defined here
  --> test.py:9:5
   |
 7 |     return None
 8 |
 9 | def take_int_only(must_be_int: int):
   |     ^^^^^^^^^^^^^ ---------------- Parameter declared here
10 |     pass
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

#### Expected Behavior

Ty should recognize that after the conditional block, the variable `int_or_none` can only be of type `int` and should not raise an error.

For comparison, Pyright correctly handles this control flow and reports no errors:

```shell
$ pyright test.py
0 errors, 0 warnings, 0 informations
```

#### Command and Settings

The command used was `uvx ty check <filename>.py` with default settings.

#### Playground Link

This issue can be reproduced in the Ty playground here:

[https://play.ty.dev/eba9c0b6-0443-492f-be34-b8eea7d795c2](https://play.ty.dev/eba9c0b6-0443-492f-be34-b8eea7d795c2)

### Version

ty 0.0.1-alpha.12

---

_Comment by @erictraut on 2025-06-27 20:32_

The feature you're asking for here is referred to as [aliased conditional expressions](https://microsoft.github.io/pyright/#/type-concepts-advanced?id=aliased-conditional-expression). Mypy and pyrefly don't have this feature either. When I implemented this feature in pyright, I borrowed the idea from the TypeScript compiler.

---

_Label `narrowing` added by @carljm on 2025-06-27 20:34_

---

_Comment by @carljm on 2025-06-27 20:36_

Thanks for the very clearly written report! I don't think this will be a high priority in the short term, but it would be nice to add it.

It's also important for the implementation to ensure that `is_none_or_int` can't have been written between the assignment to `is_none` and its use (pyright seems to do this well.)

---

_Comment by @InSyncWithFoo on 2025-06-28 04:14_

A possible approach is to infer `TypeIs[None @ variable]` for expressions of the form `variable is None`. ty doesn't support indirect `TypeIs` yet, though.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:27_

---

_Comment by @carljm on 2025-11-15 16:25_

We will need this feature, along with #1479, in order to eliminate a bunch of false positives checking the pydantic code base; see e.g. https://github.com/pydantic/pydantic/blob/f42171c760d43b9522fde513ae6e209790f7fefb/pydantic/_internal/_schema_gather.py#L94

---

_Renamed from "Type narrowing fails to account for reassignment in a conditional based on an intermediate variable" to "Type narrowing fails to account for reassignment in a conditional based on an intermediate variable (aliased conditional expressions)" by @carljm on 2025-11-15 16:35_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-15 16:36_

---

_Added to milestone `Stable` by @carljm on 2025-11-15 16:36_

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 19:41_

---
