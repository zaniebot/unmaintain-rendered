```yaml
number: 14503
title: "B024: false negative with annotated, unassigned instance variables of a class"
type: issue
state: open
author: cmp0xff
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-20T20:59:10Z
updated_at: 2024-11-20T21:13:55Z
url: https://github.com/astral-sh/ruff/issues/14503
synced_at: 2026-01-10T11:09:56Z
```

# B024: false negative with annotated, unassigned instance variables of a class

---

_Issue opened by @cmp0xff on 2024-11-20 20:59_

This issue derives from https://github.com/astral-sh/ruff/issues/14455#issuecomment-2489472624.

[copied from other issue] Meanwhile, upon checking the [Python documentation](https://docs.python.org/3/library/typing.html#typing.ClassVar), it turns out that only when subscribed by typing.ClassVar is an annotate variable considered a class variable. Copied from the above-mentioned documentation:
```python
class Starship:
    stats: ClassVar[dict[str, int]] = {} # class variable
    damage: int = 10                     # instance variable
```
What I had as an example in the original post is therefore an instance variable, not a class variable. This contradicts the discussion in https://github.com/PyCQA/flake8-bugbear/issues/293, which only has to do with typing.ClassVar.



---

_Label `rule` added by @dylwil3 on 2024-11-20 21:08_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-20 21:08_

---

_Comment by @dylwil3 on 2024-11-20 21:13_

To clarify, the suggestion here is this:

```python
from abc import ABC

class Foo(ABC):
    a: ClassVar[int]              #<--- should not trigger B024

class Bar(ABC):
    b: int              #<--- should trigger B024 because it is not annotated as a class variable
```

this makes some sense to me but I'd have to read the documentation to make sure I'm understanding correctly, so I'm not sure whether to adopt this behavior just yet.

---
