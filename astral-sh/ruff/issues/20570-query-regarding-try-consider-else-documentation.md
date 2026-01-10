```yaml
number: 20570
title: Query regarding try-consider-else documentation
type: issue
state: closed
author: LilMonk
labels:
  - documentation
assignees: []
created_at: 2025-09-25T10:47:05Z
updated_at: 2025-10-01T21:43:43Z
url: https://github.com/astral-sh/ruff/issues/20570
synced_at: 2026-01-10T11:09:59Z
```

# Query regarding try-consider-else documentation

---

_Issue opened by @LilMonk on 2025-09-25 10:47_

### Question

In the documentation https://docs.astral.sh/ruff/rules/try-consider-else/ it says to use the pattern
```python
import logging

def reciprocal(n):
    try:
        rec = 1 / n
    except ZeroDivisionError:
        logging.exception("Exception occurred")
    else:
        print(f"reciprocal of {n} is {rec}")
        return rec
```
when I use it in my code
```python
import logging

class Bar(BaseModel):
    a: int
    b: str

def create_bar(a: int, b: str) -> Bar:
    if a < 0:
        raise ValueError("a must be non-negative")
    return Bar(a=a, b=b)

def foo() -> Bar:
    log = logging.getLogger("foo")
    log.info("Creating Bar instance")
    try:
        bar = create_bar(1, "test")
    except Exception:
        log.exception("Failed to create Bar instance")
    else:
        return bar
```

But on type checkers like pylance, it gives an error
```
Function with declared return type "Bar" must return a value on all code paths
  "None" is not assignable to "Bar"
```
It is only resolved if I have a `raise` in an except block like this
```python
import logging

class Bar(BaseModel):
    a: int
    b: str

def create_bar(a: int, b: str) -> Bar:
    if a < 0:
        raise ValueError("a must be non-negative")
    return Bar(a=a, b=b)

def foo() -> Bar:
    log = logging.getLogger("foo")
    log.info("Creating Bar instance")
    try:
        bar = create_bar(1, "test")
    except Exception:
        log.exception("Failed to create Bar instance")
        raise
    else:
        return bar
```

Is my understanding of the documentation wrong, or is there anything else that I'm missing?

### Version

ruff 0.12.12

---

_Label `question` added by @LilMonk on 2025-09-25 10:47_

---

_Comment by @ntBre on 2025-09-25 13:22_

I think the docs could probably be made more clear by returning a default value after logging the exception:

```py
import logging

def reciprocal(n):
    try:
        rec = 1 / n
    except ZeroDivisionError:
        logging.exception("Exception occurred")
        return 0
    else:
        print(f"reciprocal of {n} is {rec}")
        return rec
```

or re-raising as you mentioned. I think code like this should also run afoul of type checkers for the same reason, though:

```py
# try.py
import logging

class Bar: ...

def create_bar(a, b) -> Bar: Bar()

def foo() -> Bar:
    log = logging.getLogger("foo")
    log.info("Creating Bar instance")
    try:
        bar = create_bar(1, "test")
        return bar
    except Exception:
        log.exception("Failed to create Bar instance")
```

```shell
> pyright try.py
/tmp/tmp.AK1s99fmwX/try.py
  /tmp/tmp.AK1s99fmwX/try.py:6:25 - error: Function with declared return type "Bar" must return value on all code paths
    "None" is not assignable to "Bar" (reportReturnType)
  /tmp/tmp.AK1s99fmwX/try.py:8:14 - error: Function with declared return type "Bar" must return value on all code paths
    "None" is not assignable to "Bar" (reportReturnType)
2 errors, 0 warnings, 0 informations
```


---

_Comment by @LilMonk on 2025-09-25 14:12_

So it means the doc needs some changes, right?

---

_Comment by @ntBre on 2025-09-25 14:26_

Sure! Feel free to open a PR if you're interested :)

---

_Label `question` removed by @ntBre on 2025-09-25 14:26_

---

_Label `documentation` added by @ntBre on 2025-09-25 14:26_

---

_Comment by @LilMonk on 2025-09-26 01:09_

Sure will try to make the changes

---

_Closed by @charliermarsh on 2025-10-01 21:43_

---
