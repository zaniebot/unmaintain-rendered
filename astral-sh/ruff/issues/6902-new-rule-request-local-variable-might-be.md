```yaml
number: 6902
title: "New rule request: Local variable might be referenced before assignment"
type: issue
state: closed
author: Garrett-R
labels:
  - bug
assignees: []
created_at: 2023-08-26T19:11:40Z
updated_at: 2025-09-08T23:28:58Z
url: https://github.com/astral-sh/ruff/issues/6902
synced_at: 2026-01-12T15:54:46Z
```

# New rule request: Local variable might be referenced before assignment

---

_@Garrett-R_

It would be awesome if Ruff could catch an error (like PyCharm does) for code like this:

```python
def func(x: bool) -> int:
    if x:
        y = 1
    return y
```


---

_Label `bug` added by @charliermarsh on 2023-08-26 21:19_

---

_Comment by @charliermarsh on 2023-08-26 21:19_

Makes sense. It's not trivial to fix because it requires supporting full branch analysis, but it would be good to catch of course.

---

_Comment by @Garrett-R on 2023-08-26 22:29_

Yeah, seems really complicated, as does all this linter magic haha

---

_Comment by @inoa-jboliveira on 2023-10-23 21:21_

At least pycharm and possibly other IDEs static code analysis can catch this bug. I was also hoping to also find this rule in Ruff.
I thought F823 would catch this, but it doesn't.


---

_Comment by @clemente0731 on 2024-10-09 08:20_

SAME ISSUE FOR "Local variable might be referenced before assignment" 

---

_Comment by @clemente0731 on 2024-10-10 05:58_

https://github.com/google/pytype  can work well for this @charliermarsh 

---

_Comment by @Garrett-R on 2025-09-07 05:31_

Closing the issue — this seems to be covered by [Ty](https://github.com/astral-sh/ty)'s `possibly-unresolved-reference`!  🎊 

With this config:
```py
[tool.ty.rules]
possibly-unresolved-reference = "error"
```

and running on the above example, you get:
```py
error[possibly-unresolved-reference]: Name `y` used when possibly not defined
 --> del.py:4:12
  |
2 |     if x:
3 |         y = 1
4 |     return y
  |            ^
  |

```

---

_Closed by @Garrett-R on 2025-09-07 05:31_

---

_Comment by @inoa-jboliveira on 2025-09-07 14:22_

This seems like way more suitable for a linter than a type checker (I know I know, there's a lot of overlap).

IIUC, since both projects share most of the code base, why not add this to a ruff rule, especially since ty isn't production ready. 

---

_Comment by @Garrett-R on 2025-09-08 23:28_

I guess I could see it in both.  Linter could certainly catch that, or a typing system since the symbol at that point has an ill-defined type, so it's at least the type checker's problem too.

> why not add this to a ruff rule, especially since Ty isn't production ready

Probably best to decide where it belongs long-term and roll with that, rather than letting the temporary Alpha state of Ty be a deciding factor on where it should live.

---
