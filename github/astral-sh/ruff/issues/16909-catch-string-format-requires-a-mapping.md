---
number: 16909
title: Catch string format requires a mapping
type: issue
state: open
author: lovetox
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-03-22T07:53:29Z
updated_at: 2025-03-22T09:48:58Z
url: https://github.com/astral-sh/ruff/issues/16909
synced_at: 2026-01-07T13:12:16-06:00
---

# Catch string format requires a mapping

---

_Issue opened by @lovetox on 2025-03-22 07:53_

### Summary

Neither pylint nor ruff currently catch this.

```python
somestring = "test"
"You are now %(somestring)s of this group chat" % somestring
```

This leads to

```
Traceback (most recent call last):
  File "/home/lovetox/projects/tests/enumtest.py", line 2, in <module>
    "You are now %(somestring)s of this group chat" % somestring
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
TypeError: format requires a mapping
```



---

_Label `rule` added by @MichaReiser on 2025-03-22 09:47_

---

_Label `type-inference` added by @MichaReiser on 2025-03-22 09:47_

---

_Comment by @MichaReiser on 2025-03-22 09:48_

We could dedect the most simple cases where *somestring* is defined locally and is a known type (e.g. a string) but anything more complicated requires better type inference to know whether `somestring` is a dict or not

```py
>>> somestring ={"somestring": "test"}
... "You are now %(somestring)s of this group chat" % somestring
...
```

---
