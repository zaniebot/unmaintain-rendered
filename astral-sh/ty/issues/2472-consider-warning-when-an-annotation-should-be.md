```yaml
number: 2472
title: Consider warning when an annotation should be narrower
type: issue
state: closed
author: jack-mcivor
labels:
  - wish
  - lint
assignees: []
created_at: 2026-01-12T19:11:33Z
updated_at: 2026-01-14T16:14:34Z
url: https://github.com/astral-sh/ty/issues/2472
synced_at: 2026-01-14T16:38:48Z
```

# Consider warning when an annotation should be narrower

---

_@jack-mcivor_

I think this could catch real issues in code - for example, this currently does not error (ty v0.0.11), but is a runtime error:
```python
from typing import Sequence

def foo() -> Sequence[int]:  # <- the annotation here can be narrower - should be `tuple[Literal[5], Literal[6]]`
    return (5, 6)
foo()[2]  # bad - no ty error!
```

This variable annotation should also be made narrower. The difference here is that ty seems to ignore the annotation, so there is an error
```python
a: Sequence[int] = (5, 6)  # <- the annotation here can be narrower (and afaict unused by ty anyway)
a[2]  # good - "error[index-out-of-bounds]: Index 2 is out of bounds for tuple `tuple[Literal[5], Literal[6]]` with length 2"
```

---

_Label `wish` added by @carljm on 2026-01-12 21:39_

---

_Comment by @carljm on 2026-01-12 21:40_

Hi, thanks for the suggestion!

I think "warn when an annotation should be narrower" would likely result in a lot of noisy false positives (depending on how "should" is defined, which isn't specified here but is really the crux of the proposal). Many annotations in real-world typed code _could_ be narrower than they are, but there are often good reasons why they shouldn't be narrower. It may be that `foo()` is a method and subclasses should be allowed to return some other sequence of integers. Or it is planned to make the method more complex in the future, so the return type shouldn't make promises to callers that may be broken in future. Or the more precise annotation would be a mutable type, and it's desirable to prevent mutation. I suspect that once these types of cases are all eliminated, what's left is a rule that only applies to a few toy examples (how common is a standalone function that always returns exactly the same tuple?)

We can leave this open as a place to collect concrete cases where something like this could be useful, but I don't think it will be a priority in the short term, since this is something that no current type checker attempts to do.

In #136 we plan to change shortly to start preferring annotated types over inferred types in annotated assignments. That will make the second example behave more like the first. If that isn't desired, the workaround would be to separate the annotation from the the assignment, e.g.

```py
a: Sequence[int]
a = (5, 6)
```

Which will result in an inferred type of `tuple[Literal[5], Literal[6]]` instead of `Sequence[int]` for `a`.

---

_Label `lint` added by @AlexWaygood on 2026-01-12 21:41_

---

_Comment by @jack-mcivor on 2026-01-14 14:56_

Thanks for the response. I thought about this, and I agree with your perspectives. 

I also tried to find some cases in real code where this would be useful, but didn't find anything very compelling. There were a number of places that `Mapping` could be converted to `dict` - but I can see the Mapping type has been chosen to ban mutation (just like you mentioned). There were a few cases of `tuple[T, ...] -> tuple[T, T]` or similar, but I don't think it's a big value add.

One interesting case is suggesting `NewType`s instead of type aliases (or simple unaliased types like `int` or `MyClass`). I don't see an easy way to make this into a tool though, and your many of your points would stand here too.

---

_Comment by @carljm on 2026-01-14 16:14_

Thanks for following up! Since it seems we don't really have a compelling case for this, I think I'll close it as "not planned" for now -- we can reopen if stronger arguments for it arrive.

---

_Closed by @carljm on 2026-01-14 16:14_

---
