```yaml
number: 1989
title: Tuple return type contains incorrect union
type: issue
state: closed
author: judahrand
labels:
  - question
assignees: []
created_at: 2025-12-17T09:01:22Z
updated_at: 2025-12-17T09:08:02Z
url: https://github.com/astral-sh/ty/issues/1989
synced_at: 2026-01-10T01:55:00Z
```

# Tuple return type contains incorrect union

---

_Issue opened by @judahrand on 2025-12-17 09:01_

### Summary

I've had a quick poke through existing issues and couldn't find something that looked like this.

```python
from typing import reveal_type


def func() -> tuple[int, str, float]:
    return 42, "hello", 2.71


a, b, c = func()
reveal_type(func)  # Expected: def func() -> tuple[int, str, float]
reveal_type(a)  # Expected: int
reveal_type(b)  # Expected: str
reveal_type(c)  # Expected: float
```

`ty` seems to think that the signature of `func` is `def func() -> tuple[int, str, int | float]` despite the declared signature.

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @sharkdp on 2025-12-17 09:02_

Thank you for reporting this.

We treat an annotation of `float` as being equivalent to `int | float`, as explained in [this section](https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex) of the Python typing specification.

---

_Label `question` added by @sharkdp on 2025-12-17 09:02_

---

_Comment by @AlexWaygood on 2025-12-17 09:04_

(Hey @judahrand, great to see you here! 😃)

---

_Comment by @judahrand on 2025-12-17 09:07_

> Thank you for reporting this.
> 
> We treat an annotation of `float` as being equivalent to `int | float`, as explained in [this section](https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex) of the Python typing specification.

Aha! That explains it! Desired behaviour rather than a bug. I came across this while try to simplify another case which _seemed_ wrong to me (but is contained within a closed codebase so can't share the direct example). I'll close this report and see if I can come up with something which reproduces the other thing 😅 

> (Hey @judahrand, great to see you here! 😃)

I'm always lurking away in the background 😛 

---

_Comment by @judahrand on 2025-12-17 09:08_

> Thank you for reporting this.
> 
> We treat an annotation of `float` as being equivalent to `int | float`, as explained in [this section](https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex) of the Python typing specification.



---

_Closed by @judahrand on 2025-12-17 09:08_

---
