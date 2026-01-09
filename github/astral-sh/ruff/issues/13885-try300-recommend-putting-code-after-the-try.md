---
number: 13885
title: "TRY300: Recommend putting code after the try/except instead of inside a else"
type: issue
state: open
author: cbornet
labels:
  - fixes
assignees: []
created_at: 2024-10-23T08:33:12Z
updated_at: 2024-10-23T09:05:10Z
url: https://github.com/astral-sh/ruff/issues/13885
synced_at: 2026-01-07T13:12:16-06:00
---

# TRY300: Recommend putting code after the try/except instead of inside a else

---

_Issue opened by @cbornet on 2024-10-23 08:33_

When an exception handler re-raises or returns, it seems to me that it's better to put the "else" code as a follow-up to the try/except than in a "else".

Eg for the code
```python
import logging


def reciprocal(n):
    try:
        rec = 1 / n
        print(f"reciprocal of {n} is {rec}")
        return rec
    except ZeroDivisionError:
        logging.exception("Exception occurred")
        raise
```
you get `TRY300 Consider moving this statement to an `else` block`
but the best for me would be to write:
```python
import logging


def reciprocal(n):
    try:
        rec = 1 / n
    except ZeroDivisionError:
        logging.exception("Exception occurred")
        raise
    print(f"reciprocal of {n} is {rec}")
    return rec
```

BTW, the current example also seems incorrect as `reciprocal` returns an implicit `None` (See [RET503](https://docs.astral.sh/ruff/rules/implicit-return/) even though it doesn't detect it)

---

_Label `fixes` added by @MichaReiser on 2024-10-23 09:05_

---

_Comment by @MichaReiser on 2024-10-23 09:05_

I think that makes sense. But we have to be careful only to do so if all except branches re-raise and there's no existing `else` branch. 

Moving the code after the `try` would change the semantic in this case.

```python
def reciprocal(n):
    try:
        rec = 1 / n
        print(f"reciprocal of {n} is {rec}")
        return rec
    except ZeroDivisionError:
        logging.exception("Exception occurred")
        raise

    except Other:
        logging.exception("Exception occurred")
```

---
