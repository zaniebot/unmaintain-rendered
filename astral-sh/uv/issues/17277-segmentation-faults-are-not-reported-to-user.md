```yaml
number: 17277
title: Segmentation faults are not reported to user
type: issue
state: closed
author: romuald
labels:
  - enhancement
assignees: []
created_at: 2025-12-31T09:12:38Z
updated_at: 2026-01-02T15:29:02Z
url: https://github.com/astral-sh/uv/issues/17277
synced_at: 2026-01-12T16:02:48Z
```

# Segmentation faults are not reported to user

---

_@romuald_

### Summary

Found this out while running a test suite that crashed the python interpreter with a bus error during interpreter shutdown

uv run gives no indication that the program crashed (contrary to shell for example), which may be confusing in some specific cases

### Example

Minimalist example:

```python
import ctypes
                                                                               
ctypes.string_at(0)
```

```
% uv run python pycrash.py
% 
```
(nothing except exit code 139)

----

Minimal example in my case:

```python
import atexit
import ctypes

def test_crash():
    atexit.register(ctypes.string_at, 0)
    assert True
```

```
% uvx run pytest test_crash.py
=========== test session starts ===========
…
test_crash.py .                     [100%]

============ 1 passed in 0.00s ============
% 
```

(which gives no indication that something went wrong except the exit code)

### Expected behavior

I would expect something similar to what is shown when run directly inside a shell:

```
% .venv/bin/pytest test_crash.py
…
============ 1 passed in 0.00s ============
Segmentation fault (core dumped)
%


---

_Label `enhancement` added by @romuald on 2025-12-31 09:12_

---

_Comment by @EliteTK on 2026-01-01 01:14_

Looks like a duplicate of #11886 ?

---

_Comment by @romuald on 2026-01-02 15:29_

> Looks like a duplicate of [#11886](https://github.com/astral-sh/uv/issues/11886) ?

Absolutely, thank you!

---

_Closed by @romuald on 2026-01-02 15:29_

---
