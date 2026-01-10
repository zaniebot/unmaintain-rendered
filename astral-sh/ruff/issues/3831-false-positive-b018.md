---
number: 3831
title: "False positive: B018"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2023-03-31T16:34:55Z
updated_at: 2024-03-27T14:37:47Z
url: https://github.com/astral-sh/ruff/issues/3831
synced_at: 2026-01-10T01:22:42Z
---

# False positive: B018

---

_Issue opened by @AA-Turner on 2023-03-31 16:34_

Ruff 0.0.260

Reproducer:

```
(sphinx) PS I:\Development\sphinx> pytest -vvv bug_b018.py
========================================================================================================= test session starts =========================================================================================================
platform win32 -- Python 3.11.0, pytest-7.2.2, pluggy-1.0.0 -- I:\Mambaforge\envs\sphinx\python.exe
cachedir: .pytest_cache
rootdir: I:\Development\sphinx, configfile: pyproject.toml
plugins: xdist-3.2.0
collected 1 item                                                                                                                                                                                                                        

bug_b018.py::test_attribute_error PASSED                                                                                                                                                                                         [100%] 

========================================================================================================== 1 passed in 0.01s ========================================================================================================== 
(sphinx) PS I:\Development\sphinx> type bug_b018.py                                        
import pytest


def test_attribute_error():
    obj = object()
    with pytest.raises(AttributeError):
        obj.nonexistent
(sphinx) PS I:\Development\sphinx> ruff --isolated --show-source --select B018 bug_b018.py  
bug_b018.py:7:9: B018 Found useless expression. Either assign it to a variable or remove it.
  |
7 |         obj.nonexistent
  |         ^^^^^^^^^^^^^^^ B018
  |

Found 1 error.
(sphinx) PS I:\Development\sphinx> 
```

I think that this may be related to #3453 and #3455, as this is a regression from 0.0.259 to 0.0.260.

In tests it can be useful to check that an attribute does not exist, or some behaviour relating to side effects on accessing an attribute. Perhaps if ``unittest`` or ``pytest`` context managers are detected, B018 should not be raised?

Thanks,
Adam

---

_Comment by @charliermarsh on 2023-03-31 17:11_

Yeah I considered not flagging _any_ expressions that use attributes or subscript accesses as "useless". It would've limited the rule significantly, but it also would help us avoid a class of false-positives...

---

_Label `bug` added by @charliermarsh on 2023-03-31 17:12_

---

_Comment by @leiserfg on 2023-04-01 21:49_

I will prefer if it stays as is, it is better having to add a `noqa` than having a dangling attribute reference, that either means it's a non-required expression or is a descriptor with secondary effects. The first one should be deleted, the second one should be avoided and both require human intervention.

---

_Comment by @AA-Turner on 2023-04-01 23:26_

> ... it is better having to add a `noqa` than having a dangling attribute reference

I would tend to agree in the general case, my specific request was that:

> if `unittest` or `pytest` context managers are detected, B018 should not be raised

Would you agree with this, specifically in a testing context?

Thanks,
Adam

---

_Comment by @charliermarsh on 2023-04-02 02:20_

I think the request is reasonable but I'm hesitant to act on it. I see it as similarly to how we handle unused imports within `ModuleNotFoundError`-like try-except blocks. We don't _ignore_ them, since it can be surprising to users and hard to understand the intent explicitly. But we do have a custom message that points users in the right direction for a more preferable pattern. 

In this case, my preference would be to encourage a pattern like:

```py
with blah blah:
    _ = obj.nonexistent
```

To me, that signals the intent that the variable is intentionally unused (and IIRC we should avoid flagging that as an unused local).


---

_Referenced in [astropy/astropy#14692](../../astropy/astropy/pulls/14692.md) on 2023-04-26 17:09_

---

_Referenced in [astral-sh/ruff#5006](../../astral-sh/ruff/issues/5006.md) on 2023-06-10 14:57_

---

_Comment by @spaceone on 2023-06-10 15:05_

`_` is often used as gettext translation variable.

---

_Comment by @leiserfg on 2023-06-10 22:27_

It can be any var matching "dummy-variable-rgx" like `_noqa = obj.nonexistent`.

---

_Referenced in [astral-sh/ruff#9082](../../astral-sh/ruff/issues/9082.md) on 2023-12-10 23:34_

---

_Referenced in [pytest-dev/pytest#11914](../../pytest-dev/pytest/pulls/11914.md) on 2024-02-02 21:13_

---

_Comment by @lafrech on 2024-03-27 09:54_

I came here with the same issue and I believe the underscore suggestion is a nice solution.

---

_Comment by @charliermarsh on 2024-03-27 14:37_

Closing this for now as we don't intend to change it, and I recently added the underscore suggestion to the docs (but may not be released yet, can't remember off-hand).

---

_Closed by @charliermarsh on 2024-03-27 14:37_

---
