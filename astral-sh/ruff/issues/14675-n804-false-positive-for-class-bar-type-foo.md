```yaml
number: 14675
title: "N804 false positive for `class Bar(type(foo))`"
type: issue
state: closed
author: alexhenman
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-29T11:44:50Z
updated_at: 2024-11-30T22:37:29Z
url: https://github.com/astral-sh/ruff/issues/14675
synced_at: 2026-01-12T15:54:54Z
```

# N804 false positive for `class Bar(type(foo))`

---

_@alexhenman_

```python
foo = {}

class Bar(type(foo)):
    def foo_method(self):
        pass
```

https://play.ruff.rs/240f9b93-2408-4aeb-814d-bb09629496da

Not sure if this might be related to the issue reported in #12736 and its fix

---

_Comment by @AlexWaygood on 2024-11-29 12:37_

> Not sure if this might be related to the issue reported in [#12736](https://github.com/astral-sh/ruff/issues/12736) and its fix

yes, it is.

Hmm, not sure what to do here. This is hard to fix properly without sophisticated type inference. Maybe instead of returning `bool`, our `is_metaclass` function here should return an enum with three variants: `Yes`, `No` and `Maybe`. For classes with dynamic bases such as this class and the class reported in #12736, the function would return `Maybe`, and we would not emit either N804 or N805 on the methods in the class.

---

_Label `bug` added by @AlexWaygood on 2024-11-29 12:38_

---

_Label `help wanted` added by @MichaReiser on 2024-11-29 12:48_

---

_Comment by @AlexWaygood on 2024-11-29 13:16_

> Maybe instead of returning `bool`, our `is_metaclass` function here should return an enum with three variants: `Yes`, `No` and `Maybe`. For classes with dynamic bases such as this class and the class reported in [#12736](https://github.com/astral-sh/ruff/issues/12736), the function would return `Maybe`, and we would not emit either N804 or N805 on the methods in the class.

If anybody wants to help out with this issue: let's try doing something like this

---

_Closed by @AlexWaygood on 2024-11-30 22:37_

---
