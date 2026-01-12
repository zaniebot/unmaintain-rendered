```yaml
number: 9159
title: PEP 695 type annotations with lambdas produce false F401
type: issue
state: closed
author: rijenkii
labels:
  - bug
assignees: []
created_at: 2023-12-16T10:43:12Z
updated_at: 2023-12-18T04:51:24Z
url: https://github.com/astral-sh/ruff/issues/9159
synced_at: 2026-01-12T15:54:49Z
```

# PEP 695 type annotations with lambdas produce false F401

---

_@rijenkii_

Minimal reproduction:
```python
from typing import Annotated
import re

type X = Annotated[int, lambda: re.compile("x")]
```

Result:
```
$ ruff repro.py --isolated
repro.py:2:8: F401 [*] `re` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Settings:
```toml
[tool.ruff.lint]
select = ["F", "E"]
```

Version 0.1.8

---

_Label `bug` added by @charliermarsh on 2023-12-16 14:06_

---

_Comment by @charliermarsh on 2023-12-16 14:14_

Thanks! Agree that we should probably fix this, although I admittedly don't understand how `Annotated` is supposed to work here. For example:

```python
from typing import Annotated

type X = Annotated[int, lambda: re.compile("x")]
print(X.__metadata__)
```

Yields:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/foo.py", line 10, in <module>
    print(X.__metadata__)
          ^^^^^^^^^^^^^^
AttributeError: 'typing.TypeAliasType' object has no attribute '__metadata__'. Did you mean: '__getstate__'?
```

---

_Comment by @charliermarsh on 2023-12-16 14:15_

On the other hand, Ruff correctly does _not_ flag F401 here:

```python
from typing import Annotated

X = Annotated[int, lambda: re.compile("x")]
print(X.__metadata__)
```

So I suspect our treatment right be correct.

---

_Label `bug` removed by @charliermarsh on 2023-12-16 14:15_

---

_Label `question` added by @charliermarsh on 2023-12-16 14:15_

---

_Comment by @rijenkii on 2023-12-16 14:16_

> Thanks! Agree that we should probably fix this, although I admittedly don't understand how Annotated is supposed to work here

Here:
```python
>>> from typing import Annotated
>>> import re
>>> type X = Annotated[int, lambda: re.compile("x")]
>>> print(X.__value__.__metadata__)
(<function X.<locals>.<lambda> at 0x7f4df0747f60>,)
```

---

_Label `question` removed by @charliermarsh on 2023-12-16 14:19_

---

_Label `bug` added by @charliermarsh on 2023-12-16 14:19_

---

_Comment by @charliermarsh on 2023-12-16 14:19_

Huh, ok. I will admit that I find `Annotated` very strange but I agree that this is a false positive. Thanks.

---

_Comment by @rijenkii on 2023-12-16 14:22_

This example also produces an F401 and has no `Annotation`. I have not used it in the OP because according to pyright it is just wrong, but it works at runtime:
```python
import re
type X = lambda: re.compile("x")
```
```
$ ruff repro.py --isolated
repro.py:1:8: F401 [*] `re` imported but unused
Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```python
>>> import re
>>> type X = lambda: re.compile("x")
>>> X.__value__
<function X.<locals>.<lambda> at 0x7f6d4cb30040>
>>> X.__value__()
re.compile('x')
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-18 03:45_

---

_Closed by @charliermarsh on 2023-12-18 04:51_

---
