```yaml
number: 17518
title: Triple quoted strings are not formatted
type: issue
state: closed
author: R1kaB3rN
labels:
  - question
assignees: []
created_at: 2025-04-21T02:45:51Z
updated_at: 2025-04-21T16:44:23Z
url: https://github.com/astral-sh/ruff/issues/17518
synced_at: 2026-01-12T15:54:56Z
```

# Triple quoted strings are not formatted

---

_@R1kaB3rN_

### Summary

For variables that are assigned either a triple quoted f-string or string, the ruff formatter will not standardize the closing triple quotes for each variable.  For example:

`foo.py`:
```
foo = """

"""

bar = """

    """
```


Attempting to run `ruff format foo.py` results in `1 file left unchanged`. As a user, my expectation was for ruff to standardize all the closing triple quote's indentation level for each variable.

### Version

ruff 0.11.6 (fcd50a049 2025-04-17)

---

_Label `question` added by @MichaReiser on 2025-04-21 10:37_

---

_Comment by @MichaReiser on 2025-04-21 10:38_

Changing the indentation of the closing triple quotes changes the runtime semantics of the strings. That's why Ruff can't change the indent:

```py
>>> foo = """
...
... """
...
... bar = """
...
...     """
...
>>> print(foo)



>>> print(bar)



>>> print(len(bar))
6
>>> print(len(foo))
2
```

---

_Closed by @R1kaB3rN on 2025-04-21 16:44_

---
