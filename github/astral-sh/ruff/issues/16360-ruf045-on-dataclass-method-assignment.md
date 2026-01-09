---
number: 16360
title: RUF045 on dataclass method assignment
type: issue
state: open
author: beskep
labels:
  - question
  - rule
  - preview
assignees: []
created_at: 2025-02-25T03:21:39Z
updated_at: 2025-06-16T17:54:52Z
url: https://github.com/astral-sh/ruff/issues/16360
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF045 on dataclass method assignment

---

_Issue opened by @beskep on 2025-02-25 03:21_

### Description

ruff version: 0.9.7
command: `ruff check test.py --select RUF --preview`

```python
import functools
from collections.abc import Callable
from dataclasses import dataclass


@dataclass
class Vector:
    x: float
    y: float

    def __call__(self) -> None:
        print(self)

    # RUF045: Assignment without annotation found in dataclass body
    call = __call__

    call2: Callable[..., None] = __call__  # this makes new field

    def add(self, x: float, y: float):
        return Vector(x=self.x + x, y=self.y + y)

    # RUF045: Assignment without annotation found in dataclass body
    add_x = functools.partialmethod(add, y=0)
```

I am not sure if it's intended, but ruff check RUF045 for assigning a method of dataclass.

Is there a more appropriate way for typing new method, perhaps?

---

_Comment by @InSyncWithFoo on 2025-02-25 07:55_

@dylwil3 This was a potential problem [I noticed](https://github.com/astral-sh/ruff/pull/14349#issuecomment-2477727417). What do you think, as the approver of that PR?

---

_Label `rule` added by @dhruvmanila on 2025-02-25 10:14_

---

_Label `needs-decision` added by @dhruvmanila on 2025-02-25 10:14_

---

_Label `preview` added by @dhruvmanila on 2025-02-25 10:14_

---

_Comment by @dylwil3 on 2025-02-25 12:04_

> Is there a more appropriate way for typing new method, perhaps?

The suggestion in the documentation and "help" text of the rule is still correct: annotating the assignment with `ClassVar[Callable]` will give the behavior you're looking for.

> I am not sure if it's intended, but ruff check RUF045 for assigning a method of dataclass.

> @dylwil3 This was a potential problem [I noticed](https://github.com/astral-sh/ruff/pull/14349#issuecomment-2477727417). What do you think, as the approver of that PR?

Good questions! 

I think the lint is correct to emit here: You can have dataclass fields that are typed as `Callable` and you can give them defaults; the help text of the diagnostic message is correct as given and the presence of the lint is consistent with the reasoning given in the documentation. I don't consider this a false positive and would argue in keeping this behavior even if we had stronger type inference around to avoid it.

In the discussion of the PR for this rule, I think there was some connection drawn to [this issue around `RUF012`](https://github.com/astral-sh/ruff/issues/5243) and the comment:

> This has the (maybe desirable, maybe not, but was surprising to me) effect of forcing typing for a RUF rule, in places where you would not necessarily use typing (if you did not want to).

But I really don't think those complaints apply to `RUF045`: dataclasses _already_ enforce the use of typing, and in fact the main concern raised in that thread was about extending the scope of the rule to include classes _other_ than dataclasses.

All that being said: It seems like the situation is unclear enough that the current issue was submitted. So I'm happy to be convinced I'm wrong here, and to hear other points of view!

---

_Comment by @beskep on 2025-02-26 00:32_

`add_x: ClassVar = functools.partialmethod(add, y=0)` worked fine.
I had not thought of typing `ClassVar` for a function. How about adding this case to the example?

Or, perhaps, how about applying RUF045 only before `def` section?
It would increase false negatives though.

---

_Comment by @InSyncWithFoo on 2025-02-26 07:13_

The semantics of such a bare `ClassVar` annotation is currently unclear. Recently, [a proposal](https://github.com/python/typing/pull/1931) was made to address this. It hasn't been accepted yet, however.

---

_Comment by @beskep on 2025-02-26 08:10_

I appreciate RUF045 (I forget to type dataclass fields time to time), but `ClassVar[Callable[..., None]]` and `ClassVar[partialmethod[Vector]]` seems a bit verbose and cumbersome to me.
I'd rather mark noqa for those lines.

---

_Label `needs-decision` removed by @dylwil3 on 2025-03-10 10:07_

---

_Label `question` added by @dylwil3 on 2025-03-10 10:07_

---

_Comment by @CommentatorForAll on 2025-06-16 17:54_

I do agree with OP here, from my understanding, partialmethod returns a function that requires `self`. Hence the assigned variable is a callable on the object not the class. Why does one need to annotate it as a classvar then?

---
