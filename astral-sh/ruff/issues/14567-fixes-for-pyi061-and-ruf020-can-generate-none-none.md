```yaml
number: 14567
title: "Fixes for PYI061 and RUF020 can generate `None | None`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2024-11-24T22:32:47Z
updated_at: 2024-11-25T17:35:08Z
url: https://github.com/astral-sh/ruff/issues/14567
synced_at: 2026-01-10T11:09:56Z
```

# Fixes for PYI061 and RUF020 can generate `None | None`

---

_Issue opened by @dscorbett on 2024-11-24 22:32_

The fixes for [`redundant-none-literal` (PYI061)](https://docs.astral.sh/ruff/rules/redundant-none-literal/) and [`never-union` (RUF020)](https://docs.astral.sh/ruff/rules/never-union/) in Ruff 0.8.0 can generate `None | None`, which raises an error at runtime.

PYI061 example:
```console
$ cat pyi061.py
from typing import Literal
x: Literal[None] | None

$ ruff check --isolated --preview --select PYI061 pyi061.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat pyi061.py
from typing import Literal
x: None | None

$ python pyi061.py 
Traceback (most recent call last):
  File "pyi061.py", line 2, in <module>
    x: None | None
       ~~~~~^~~~~~
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'
```

RUF020 example:
```console
$ cat ruf020.py
from typing import Never
x: None | Never | None

$ ruff check --isolated --select RUF020 ruf020.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf020.py
from typing import Never
x: None | None

$ python ruf020.py
Traceback (most recent call last):
  File "ruf020.py", line 2, in <module>
    x: None | None
       ~~~~~^~~~~~
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'
```

---

_Comment by @AlexWaygood on 2024-11-24 22:44_

Hah, great catch (as ever). For reference, the CPython issue where it's debated whether we should change this behaviour at runtime in Python is https://github.com/python/cpython/issues/107271.

Regardless of whether we change it in a future version of CPython, however, this is clearly a bug in ruff, since the code does not raise an exception prior to the autofix but does afterwards.

---

_Label `bug` added by @AlexWaygood on 2024-11-24 22:44_

---

_Label `fixes` added by @AlexWaygood on 2024-11-24 22:44_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-24 22:44_

---

_Comment by @MichaReiser on 2024-11-25 08:04_

CC: @sbrugman (there's no expectation that you fix the issue, I just thought that you might be interested knowing about it)

---

_Comment by @sbrugman on 2024-11-25 13:17_

Nice catch. I'll look into it.

---

_Comment by @AlexWaygood on 2024-11-25 13:20_

The frustrating thing here is that if you have [PYI016](https://docs.astral.sh/ruff/rules/duplicate-union-member/) also enabled, then this isn't an issue. But of course we can't mandate that users select rules in tandem like that.

---

_Comment by @sbrugman on 2024-11-25 13:35_

I'm exploring to fix this in a single go: if the parent union already contains the `None` literal, then we can change the edit to `range_deletion` instead of `range_replacement`.

---

_Closed by @MichaReiser on 2024-11-25 17:35_

---
