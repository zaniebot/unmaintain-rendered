```yaml
number: 2174
title: "control flow modeling for `exit`"
type: issue
state: closed
author: Garrett-R
labels: []
assignees: []
created_at: 2025-12-22T23:10:22Z
updated_at: 2025-12-22T23:13:57Z
url: https://github.com/astral-sh/ty/issues/2174
synced_at: 2026-01-10T01:56:41Z
```

# control flow modeling for `exit`

---

_Issue opened by @Garrett-R on 2025-12-22 23:10_

### Summary

Ty does not understand that `exit(1)` ends the program (it does model `return` and `raise` correctly though).

```py
def foo() -> int | None:
    return 1

def bar() -> None:
    y = foo()
    if y is None:
        # return  # <--- This works
        # raise Exception('y is None')  # <--- This works
        exit(1)
    x: int = y
    print(x)
```
([playground](https://play.ty.dev/79235f32-377e-4e27-97e7-3ead66d37518))

This should raise no errors, but it raises:

```py
10 |     x: int = y
   |        ---   ^ Incompatible value of type `int | None`
   |        |
   |        Declared type

``


### Version

ty 0.0.5

---

_Comment by @AlexWaygood on 2025-12-22 23:11_

Thanks! Please see #690 

---

_Closed by @AlexWaygood on 2025-12-22 23:11_

---

_Comment by @Garrett-R on 2025-12-22 23:13_

My bad, was coming back to say the same thing.  Will check closed issues next time (found another duplicate after filing). 

---
