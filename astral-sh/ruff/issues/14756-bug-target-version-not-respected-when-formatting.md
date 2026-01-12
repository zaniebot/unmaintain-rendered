```yaml
number: 14756
title: "bug: `target-version` not respected when formatting `with` statement"
type: issue
state: closed
author: ziadkh0
labels:
  - question
  - formatter
assignees: []
created_at: 2024-12-03T12:44:38Z
updated_at: 2024-12-03T17:39:21Z
url: https://github.com/astral-sh/ruff/issues/14756
synced_at: 2026-01-12T15:54:54Z
```

# bug: `target-version` not respected when formatting `with` statement

---

_@ziadkh0_

Formatting the following code with `target-version` set to `"py39"` and `line-length` set to `10` (just for the example):
```py3
with A() as a, B() as b:
    pass
```

Expected output similar to:
```py3
with \
    A() as a, \
    B() as b \
:
    pass
```

Actual output:
```py3
with (
    A() as a,
    B() as b,
):
    pass
```
[Playground](https://play.ruff.rs/0186c6bc-7e53-49a5-8cd7-f9a4dfe48109)

Support for using grouping parentheses to break the `with` statement in multiple lines was added in version 3.10, see: https://docs.python.org/3/reference/compound_stmts.html#the-with-statement


---

_Comment by @AlexWaygood on 2024-12-03 12:56_

Thanks for opening the issue!

> Support for using grouping parentheses to break the `with` statement in multiple lines was added in version 3.10, see: https://docs.python.org/3/reference/compound_stmts.html#the-with-statement

It was only formally added to the language spec in Python 3.10. However, it was (initially only accidentally) made valid syntax in Python 3.9. This definitely isn't going to be reverted in CPython, so it's entirely safe to assume that `with` items can be parenthesized on Python 3.9. Black also uses parenthesized `with` items on Python 3.9, and I think it's good for us to be consistent with Black here. (Actually this _was_ recently discussed again at https://github.com/psf/black/pull/4488, but they decided to keep their current behaviour.)

Is this behaviour causing an issue for you somewhere? :-)

---

_Label `question` added by @AlexWaygood on 2024-12-03 12:56_

---

_Label `formatter` added by @AlexWaygood on 2024-12-03 12:56_

---

_Comment by @ziadkh0 on 2024-12-03 17:35_

Thank you for the quick reply!
I didn't that 😮
I just check it, and I can confirm that it's not causing an issues 😄
I guess this issue can be closed

---

_Closed by @MichaReiser on 2024-12-03 17:39_

---
