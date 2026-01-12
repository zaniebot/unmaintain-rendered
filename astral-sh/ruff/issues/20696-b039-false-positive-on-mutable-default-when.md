```yaml
number: 20696
title: "[B039] False-positive on mutable default when ContextVar has immutable type"
type: issue
state: open
author: brandonchinn178
labels:
  - question
assignees: []
created_at: 2025-10-04T06:56:20Z
updated_at: 2025-10-06T14:57:36Z
url: https://github.com/astral-sh/ruff/issues/20696
synced_at: 2026-01-12T15:54:57Z
```

# [B039] False-positive on mutable default when ContextVar has immutable type

---

_@brandonchinn178_

### Summary

The following code fails linting:
```python
FOO: ContextVar[Sequence[str]] = ContextVar("foo", default=[])
```
But this should be safe because call-sites cannot mutate it since it's a `Sequence`

---

_Comment by @ntBre on 2025-10-06 13:32_

Thanks for the report!

I'm not totally sure if we should consider this a false positive. Not being able to append to the `Sequence` seems like an invariant that only a type-checker could enforce, and at runtime it still seems possible to mutate the shared copy:

```pycon
>>> from contextvars import ContextVar
>>> from collections.abc import Sequence
>>> FOO: ContextVar[Sequence[str]] = ContextVar("foo", default=[])
>>>
>>> FOO.get()
[]
>>> FOO.get().append(1)
>>> FOO.get()
[1]
>>>
```

But I'm certainly open to other opinions!

---

_Label `question` added by @ntBre on 2025-10-06 13:32_

---

_Comment by @brandonchinn178 on 2025-10-06 14:54_

But isn't that always true? For example, ruff allows the following with B006:

```py
def foo (x: Sequence[int] = [])
```

---

_Comment by @ntBre on 2025-10-06 14:57_

Oh you're right. We already have an `is_immutable_annotation` that covers `Sequence` and that we use in B006. Maybe we should be using that here as well.

---
