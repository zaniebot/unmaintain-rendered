```yaml
number: 646
title: "ty doesn't recognize `Path.joinpath`"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2025-06-13T11:16:39Z
updated_at: 2025-06-13T14:49:38Z
url: https://github.com/astral-sh/ty/issues/646
synced_at: 2026-01-12T15:54:23Z
```

# ty doesn't recognize `Path.joinpath`

---

_@konstin_

### Summary

ty fails to recognize that `Path().joinpath("foo")` is a `Path`, just like `Path`.

The code below fails to typecheck with `Path()`, but once I use `Path().joinpath("foo")`, the type error is not caught anymore.

```python
from pathlib import Path


def run_test(uv: str) -> bool:
    return True


run_test(Path().joinpath("foo"))
```

https://play.ty.dev/3568d76c-ab51-46d5-a234-0f039b79727b

### Version

ty 0.0.1-alpha.9

---

_Comment by @AlexWaygood on 2025-06-13 11:45_

Thanks! The `joinpath` method returns `Self` in typeshed, so I think this is just https://github.com/astral-sh/ty/issues/159

---

_Closed by @AlexWaygood on 2025-06-13 11:45_

---

_Comment by @carljm on 2025-06-13 14:45_

> I think this is just [#159](https://github.com/astral-sh/ty/issues/159)

But I think we already support explicit `Self` annotations -- what we're lacking is just implicit understanding of `Self` as the type of unannotated `self`. So I don't think it makes sense that this should be part of #159 

---

_Comment by @AlexWaygood on 2025-06-13 14:47_

It's the same as `Bar.method3()` in https://github.com/astral-sh/ty/issues/159#issuecomment-2866084949

---

_Comment by @carljm on 2025-06-13 14:49_

Hmm I didn't realize that our support for returning `Self` was limited in that way! That's a bit surprising.

---
