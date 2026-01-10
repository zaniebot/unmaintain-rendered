```yaml
number: 484
title: Type analyze error with pydantic base model instance
type: issue
state: closed
author: birdhackor
labels: []
assignees: []
created_at: 2025-05-22T08:08:30Z
updated_at: 2025-05-22T08:50:55Z
url: https://github.com/astral-sh/ty/issues/484
synced_at: 2026-01-10T02:34:10Z
```

# Type analyze error with pydantic base model instance

---

_Issue opened by @birdhackor on 2025-05-22 08:08_

### Summary

```
from pydantic import BaseModel, Field

class D(BaseModel):
    target_tests: str | None = Field(None, alias="targetTests")

def f1(d: D):
    if d.target_tests is None:
        raise RuntimeError
    tt = p(d.target_tests)

def p(v: str):
    print(v)
```

Check result:

```
error[invalid-argument-type]: Argument to function `p` is incorrect
  --> test_ty.py:9:12
   |
 7 |     if d.target_tests is None:
 8 |         raise RuntimeError
 9 |     tt = p(d.target_tests)
   |            ^^^^^^^^^^^^^^ Expected `str`, found `str | None`
10 |
11 | def p(v: str):
   |
info: Function defined here
  --> test_ty.py:11:5
   |
 9 |     tt = p(d.target_tests)
10 |
11 | def p(v: str):
   |     ^ ------ Parameter declared here
12 |     print(v)
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

In this case, `d.target_tests` could only be `str`.


If code modify to this, it will pass.

```
def f1(d: str | None = None):
    if d is None:
        raise RuntimeError
    tt = p(d)

def p(v: str):
    print(v)
```

### Version

0.0.1-alpha.6

---

_Comment by @MichaReiser on 2025-05-22 08:40_

I think this is another instance of https://github.com/astral-sh/ty/issues/164

---

_Comment by @birdhackor on 2025-05-22 08:50_

> I think this is another instance of [#164](https://github.com/astral-sh/ty/issues/164)

Oh... you are right. I will close this issue directly.

---

_Closed by @birdhackor on 2025-05-22 08:50_

---
