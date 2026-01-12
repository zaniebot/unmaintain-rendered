```yaml
number: 1648
title: "`Literal` not infered as `list[Literal]` after casting it inside a list"
type: issue
state: closed
author: mflova
labels:
  - bidirectional inference
assignees: []
created_at: 2025-11-26T17:48:09Z
updated_at: 2025-12-20T02:17:14Z
url: https://github.com/astral-sh/ty/issues/1648
synced_at: 2026-01-12T15:54:25Z
```

# `Literal` not infered as `list[Literal]` after casting it inside a list

---

_@mflova_

### Summary

Errors not raised by `mypy` nor `pyright`

It seems that for this specific case, doing `[tags]` infers `list[str]` instead of `list[Tags]`, which makes the assignment invalid.

```py
from collections.abc import Sequence
from typing import Literal, TypeAlias

Tags: TypeAlias = Literal["a", "b"]

def func(tags: Tags | Sequence[Tags]) -> None:
    tags = [tags] if isinstance(tags, str) else tags  # error[invalid-assignment]: Object of type `list[Unknown | str] | (Sequence[Literal["a", "b"]] & ~str)` is not assignable to `Literal["a", "b"] | Sequence[Literal["a", "b"]]`
```

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25)

---

_Comment by @carljm on 2025-11-26 17:53_

Yes, looks like this is a case where we're doing literal promotion and the type context should prevent that; we probably aren't seeing through the union. Thanks for the report!

---

_Label `bidirectional inference` added by @carljm on 2025-11-26 17:53_

---

_Added to milestone `Stable` by @carljm on 2025-11-26 18:00_

---

_Comment by @ibraheemdev on 2025-11-26 23:58_

This seems to work if the annotation uses `list` instead of `Sequence`, so I think this is another instance of https://github.com/astral-sh/ty/issues/1576.

---

_Closed by @ibraheemdev on 2025-11-26 23:58_

---

_Comment by @carljm on 2025-12-20 01:52_

Reopening this so we can consider it separately from #1576, since https://github.com/astral-sh/ruff/pull/21930 doesn't seem to be sufficient to fix it.

---

_Reopened by @carljm on 2025-12-20 01:52_

---

_Comment by @carljm on 2025-12-20 01:55_

> This seems to work if the annotation uses `list` instead of `Sequence`

I think part of the reason for this is that `str` is disjoint from `list[Literal["a", "b"]]`, but not disjoint from `Sequence[Literal["a", "b"]]` (since a `str` is also a sequence of `str`, but is not a `list`). So `if isinstance(tags, str)` can narrow very effectively in the `list` case (down to just `Literal["a", "b"]`), but in the `Sequence` case it can't (the type of `tags` becomes `Literal["a", "b"] | (Sequence[Literal["a", "b"]] & str)`)

---

_Comment by @carljm on 2025-12-20 02:02_

Hmm, I think ty is actually finding a bug in the OP code here?

The string "ababab" is a `Sequence[Literal["a", "b"]]`, so it should be allowed to pass that string to `func` as annotated. And if that string is passed to `func`, it won't behave as expected: `tags` will end up as `["ababab"]` at runtime, which violates the type `Tags | Sequence[Tags]`.

Currently no type checker, including ty, actually makes `Literal["ababab"]` assignable to `Sequence[Literal["a", "b"]]` -- but I think this is a bug, it absolutely should be a subtype.

---

_Comment by @carljm on 2025-12-20 02:17_

Closing this issue, since I believe that ty is correct here. I think to be sound, the OP code either needs to use a more concrete container type (which is disjoint from `str`) instead of `Sequence`, or else needs to use the much more specific check `if tags is "a" or tags is "b"` in place of `isinstance(tags, str)`. (Note that even `tags == "a" or tags == "b"` doesn't work, since `Sequence` can include types with arbitrary `__eq__` behavior).

This latter form also does fix the example, with https://github.com/astral-sh/ruff/pull/21930:

```py
from collections.abc import Sequence
from typing import Literal, TypeAlias

Tags: TypeAlias = Literal["a", "b"]

def func(tags: Tags | Sequence[Tags]) -> None:
    tags = [tags] if tags is "a" or tags is "b" else tags
```

(Perhaps more realistically, a function returning `TypeIs` could encapsulate the "is it a `Tags`" check.)

Re-closing as duplicate of #1576, since I think with that fixed, ty is otherwise behaving as expected here.

---

_Closed by @carljm on 2025-12-20 02:17_

---
