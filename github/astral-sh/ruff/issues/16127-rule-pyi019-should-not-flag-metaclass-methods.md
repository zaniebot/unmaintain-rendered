---
number: 16127
title: "Rule `PYI019` should not flag metaclass methods"
type: issue
state: closed
author: Geo5
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-02-12T21:54:16Z
updated_at: 2025-02-13T18:44:12Z
url: https://github.com/astral-sh/ruff/issues/16127
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule `PYI019` should not flag metaclass methods

---

_Issue opened by @Geo5 on 2025-02-12 21:54_

### Description

This code which includes a metaclass and passes `mypy` without errors:
```python
from typing import TypeVar

F = TypeVar("F")


class FooMeta(type):
    def bar(cls: F) -> F:
        return cls
```
is fixed to:
```bash
ruff check --isolated --select PYI --preview --fix --diff
```
```diff
--- test.py
+++ test.py
@@ -1,8 +1,9 @@
 from typing import TypeVar
+from typing_extensions import Self
 
 F = TypeVar("F")
 
 
 class FooMeta(type):
-    def bar(cls: F) -> F:
+    def bar(cls) -> Self:
         return cls

Would fix 1 error.
```
Which `mypy` does not like:
```bash
$ mypy --strict test.py 
test.py:8: error: Self type cannot be used in a metaclass  [misc]
Found 1 error in 1 file (checked 1 source file)
```
--------
```bash
$ python --version
Python 3.11.10
$ ruff --version
ruff 0.9.6
$ mypy --version
mypy 1.15.0 (compiled: yes)
```



---

_Label `bug` added by @ntBre on 2025-02-12 22:02_

---

_Label `fixes` added by @ntBre on 2025-02-12 22:02_

---

_Comment by @Geo5 on 2025-02-12 22:02_

When reading further i am actually not sure if this is a bug in ruff or in mypy itself, because they seem to have almost this exact example in their documentation:
https://mypy.readthedocs.io/en/stable/metaclasses.html
Which also gives the same error for me.
Should i file a bug report on their side?

---

_Comment by @Geo5 on 2025-02-12 22:05_

See also https://github.com/astral-sh/ruff/issues/8353 for a similar case where the relevant PEP is linked: https://peps.python.org/pep-0673/#valid-locations-for-self
Seems like the fix is indeed incorrect and the mypy docs example seems to be incorrect aswell

---

_Comment by @ntBre on 2025-02-12 22:12_

Thanks for the report and the for the additional links! I agree that it looks like a bug in ruff based on the PEP.

---

_Comment by @Geo5 on 2025-02-12 22:23_

(I also created an issue for the mypy documentation linked above https://github.com/python/mypy/issues/18668)

---

_Comment by @AlexWaygood on 2025-02-12 22:44_

Yup, this is a bug here -- thanks for the report @Geo5! For some reason we check whether the class is a metaclass for PYI034 before issuing a diagnostic:

https://github.com/astral-sh/ruff/blob/f8093b65ea88eb11a0d0f4ffdbf7c6f007043238/crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs#L133-L136

but not for PYI019.

Should be easy to fix!

---

_Label `fixes` removed by @AlexWaygood on 2025-02-12 22:44_

---

_Label `rule` added by @AlexWaygood on 2025-02-12 22:44_

---

_Label `help wanted` added by @AlexWaygood on 2025-02-12 22:44_

---

_Renamed from "Rule `PYI019` incorrect autofix for metaclasses" to "Rule `PYI019` should not flag metaclass methods" by @AlexWaygood on 2025-02-12 23:52_

---

_Comment by @vladNed on 2025-02-13 15:13_

is this issue still unassigned ? I would like to give it a go 

---

_Comment by @AlexWaygood on 2025-02-13 15:17_

@vladNed go for it!

---

_Assigned to @vladNed by @AlexWaygood on 2025-02-13 15:17_

---

_Referenced in [astral-sh/ruff#16141](../../astral-sh/ruff/pulls/16141.md) on 2025-02-13 17:31_

---

_Closed by @AlexWaygood on 2025-02-13 18:44_

---
