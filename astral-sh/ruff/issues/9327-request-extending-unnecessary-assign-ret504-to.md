---
number: 9327
title: "Request: extending `unnecessary-assign` (`RET504`) to account for `+=` (`__iadd__`)"
type: issue
state: open
author: jamesbraza
labels:
  - rule
assignees: []
created_at: 2023-12-31T05:17:54Z
updated_at: 2024-01-02T14:20:44Z
url: https://github.com/astral-sh/ruff/issues/9327
synced_at: 2026-01-10T01:22:49Z
---

# Request: extending `unnecessary-assign` (`RET504`) to account for `+=` (`__iadd__`)

---

_Issue opened by @jamesbraza on 2023-12-31 05:17_

With `ruff==0.1.9`'s [`unnecessary-assign`](https://docs.astral.sh/ruff/rules/unnecessary-assign/) (`RET504`), this goes undetected as having extra steps:

```python
def foo(arg: int) -> int:
    arg += 1
    return arg
```

It would be cool to extend this rule to request:

```python
def foo(arg: int) -> int:
    return arg + 1
```

There is a side effect here, it moves from invoking `__iadd__` to `__add__`. So perhaps an autofix for this should be considered an "unsafe" fix

---

_Label `rule` added by @charliermarsh on 2023-12-31 12:41_

---

_Comment by @evanrittenhouse on 2023-12-31 22:13_

Feel free to assign this to me


---

_Assigned to @evanrittenhouse by @zanieb on 2023-12-31 22:16_

---

_Referenced in [astral-sh/ruff#9348](../../astral-sh/ruff/pulls/9348.md) on 2024-01-01 16:06_

---

_Comment by @Skylion007 on 2024-01-01 17:30_

I am linking a comment I made on the PR about how this may lead to undesirable behavior / unsafe edits if the return value is mutable: https://github.com/astral-sh/ruff/pull/9348#issuecomment-1873410403

---

_Comment by @jamesbraza on 2024-01-02 04:04_

Yeah thanks for pointing that out. `__iadd__` can return an input mutable arg, while `__add__` will return a copy. I had alluded to this in the original post, thanks for making it more explicit.

I think a good approach is:

- Immutable return: autofix is "safe"
- Otherwise: autofix is "unsafe"

It looks like that's what @evanrittenhouse sort of proposed in his PR (and thanks for making the PR 👍)

---

_Comment by @Skylion007 on 2024-01-02 04:07_

Yeah, there is a question of fix safety, but there is also a question of false positives here since they may entirely different ops that could have different behaviors more subtly (even without side effects).

---

_Comment by @jamesbraza on 2024-01-02 04:22_

> entirely different ops that could have different behaviors more subtly (even without side effects)

@Skylion007 I am not following what you meant by this, can you give an example?

Also, you do bring up the idea of other ops. It would be good to have this RET504 extension apply to other ops like `*=`, `-=`, `/=`, etc. May be worth adding to the test cases for that PR

---

_Comment by @Skylion007 on 2024-01-02 05:09_

Well, for instance, it's rare but a class have `__iadd__` implemented but not `__add__. And I was referring to the other inplace operator variants.

---

_Comment by @evanrittenhouse on 2024-01-02 14:20_

> other ops like *=, -=, /=, etc

FWIW this operates on the `StmtAugAssign` node, so those operators will be handled automatically

---
