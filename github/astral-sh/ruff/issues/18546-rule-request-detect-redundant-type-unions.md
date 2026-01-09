---
number: 18546
title: "Rule request: detect redundant type unions"
type: issue
state: closed
author: robsdedude
labels: []
assignees: []
created_at: 2025-06-08T08:23:47Z
updated_at: 2025-06-08T10:24:40Z
url: https://github.com/astral-sh/ruff/issues/18546
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule request: detect redundant type unions

---

_Issue opened by @robsdedude on 2025-06-08 08:23_

### Summary

Inspired by https://github.com/astral-sh/ruff/issues/18508

I'd like to see ruff detect (and possibly fix) redundant types in unions. Examples:

```python
import typing

foo1: A | B | A  # becomes A | B
foo2: typing.Union[int, B, float, int, C]  # becomes Union[int, B, float, C]
foo3: typing.Optional[None]  # becomes None
```
For simplicity, I'd be fine leaving out nested annotations in the first iteration. They could still be added at a later time. E.g.,
```python
import typing

foo: typing.Union[int, None, typing.Optional[int]]  # could become typing.Union[int, None]
```

---

_Comment by @AlexWaygood on 2025-06-08 09:57_

Thanks! We already have this one: https://docs.astral.sh/ruff/rules/duplicate-union-member/#flake8-pyi-pyi

---

_Closed by @AlexWaygood on 2025-06-08 09:57_

---

_Comment by @robsdedude on 2025-06-08 10:18_

Ah, nice! I've not checked it yet but judging by the linked issue that rule does not cover `Optional[None]`. Would you be interested in a PR to change that?

---

_Comment by @AlexWaygood on 2025-06-08 10:24_

Sure, that sounds in the spirit of the rule!

---

_Comment by @AlexWaygood on 2025-06-08 10:24_

Might have to be a preview-only change initially though, since it's an expansion in scope

---

_Referenced in [astral-sh/ruff#18572](../../astral-sh/ruff/pulls/18572.md) on 2025-06-08 20:45_

---
