```yaml
number: 2486
title: Avoid widening or promoting comprehension values
type: issue
state: closed
author: ibraheemdev
labels:
  - bug
assignees: []
created_at: 2026-01-14T01:41:36Z
updated_at: 2026-01-14T01:55:43Z
url: https://github.com/astral-sh/ty/issues/2486
synced_at: 2026-01-14T02:32:28Z
```

# Avoid widening or promoting comprehension values

---

_@ibraheemdev_

Even after https://github.com/astral-sh/ty/issues/1602 is resolved, we may still fail to infer comprehensions that iterate over a list literal, e.g.,
```py
# revealed: Unknown | str
x = [reveal_type(string) for string in ["a", "b"]]
```

We should always avoid literal promotion and `Unknown` widening here, as the list is immutable.

---

_Added to milestone `Pre-stable 1` by @ibraheemdev on 2026-01-14 01:42_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-14 01:42_

---

_Label `bug` added by @ibraheemdev on 2026-01-14 01:43_

---

_Comment by @carljm on 2026-01-14 01:50_

Other type checkers do promote here.

I think https://github.com/astral-sh/ty/issues/2441 maybe already captures the core idea here, that we can avoid promotion for single-use lists (in this case that's the inner `["a", "b"]`). 

But I'm not sure the ultimate effect of inferring the outer comprehension result as `list[Literal["a", "b"]]` would be desirable. That's a normal mutable list that subsequent code might try to add things to (although I guess if you're using comprehension/functional style that would be a slightly weird thing to do.)

---

_Comment by @ibraheemdev on 2026-01-14 01:53_

Ah I missed https://github.com/astral-sh/ty/issues/2441, I think that covers this issue.

> But I'm not sure the ultimate effect of inferring the outer comprehension result as list[Literal["a", "b"]] would be desirable

This would only apply to the inner list, the outer list would be promoted as usual to `list[str]` (or `list[Unknown | str]`. The difference is that the element type within the comprehension is `Literal["a", "b"]`, which would avoid issues like https://github.com/astral-sh/ty/issues/2355.

---

_Closed by @ibraheemdev on 2026-01-14 01:55_

---
