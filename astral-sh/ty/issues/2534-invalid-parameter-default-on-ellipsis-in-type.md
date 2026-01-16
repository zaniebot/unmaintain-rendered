```yaml
number: 2534
title: "[invalid-parameter-default] on ellipsis in `TYPE_CHECKING` section"
type: issue
state: closed
author: robsdedude
labels: []
assignees: []
created_at: 2026-01-16T14:44:08Z
updated_at: 2026-01-16T16:32:40Z
url: https://github.com/astral-sh/ty/issues/2534
synced_at: 2026-01-16T16:59:27Z
```

# [invalid-parameter-default] on ellipsis in `TYPE_CHECKING` section

---

_@robsdedude_

### Summary

https://peps.python.org/pep-0484/#arbitrary-argument-lists-and-default-argument-values reads

> In stubs it may be useful to declare an argument as having a default without specifying the actual default value. For example:
>
> ```python
> def foo(x: AnyStr, y: AnyStr = ...) -> AnyStr: ...
> ```

I argue, in the same vein as https://github.com/astral-sh/ty/issues/339, that `if TYPE_CHECKING` sections are very stub-file-like. Therefore, I suggest to allow assigning `...` (ellipsis) as a parameter default value. At least `mypy` is also fine with this.

Admittedly, mypy just allows ellipsis to be a parameter default everywhere. I'm not sure that's a good idea, really. mypy gist: https://gist.github.com/mypy-play/41532e8350478fd1a0b4fa6ee764171b

### Version

_No response_

---

_Comment by @AlexWaygood on 2026-01-16 14:47_

yeah fair enough! This is easy to fix

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-16 14:47_

---

_Added to milestone `Stable` by @carljm on 2026-01-16 15:25_

---

_Closed by @AlexWaygood on 2026-01-16 16:32_

---
