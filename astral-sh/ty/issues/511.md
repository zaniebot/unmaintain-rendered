```yaml
number: 511
title: "False positive for `PEP 604 union syntax is only available in Python 3.10 and later` when `requires-python = \">=3.9\"`"
type: issue
state: closed
author: karlicoss
labels:
  - bug
  - unreachable-code
assignees: []
created_at: 2025-05-26T00:50:40Z
updated_at: 2025-05-26T11:42:03Z
url: https://github.com/astral-sh/ty/issues/511
synced_at: 2026-01-10T02:34:10Z
```

# False positive for `PEP 604 union syntax is only available in Python 3.10 and later` when `requires-python = ">=3.9"`

---

_Issue opened by @karlicoss on 2025-05-26 00:50_

### Summary

I have `requires-python = ">=3.9"` in my `pyproject.toml` and the following bit of code
```python
import sys
from typing import Union

if sys.version_info >= (3, 10):
    T = str | None
else:
    T = Union[str, None]

print(T)
```

# current ty behaviour
It seems like the conditional for `sys.version_info` isn't handled correctly? 
```
$ uvx ty check test_310_union.py 
error[unsupported-operator]: Operator `|` is unsupported between objects of type `<class 'str'>` and `None`
 --> test_310_union.py:5:9
  |
4 | if sys.version_info >= (3, 10):
5 |     T = str | None
  |         ^^^^^^^^^^
6 | else:
7 |     T = Union[str, None]
  |
info: Note that `X | Y` PEP 604 union syntax is only available in Python 3.10 and later
info: The inferred target version of your project is Python 3.9
info: If using a pyproject.toml file, consider adjusting the `project.requires-python` or `tool.ty.environment.python-version` field
info: rule `unsupported-operator` is enabled by default
```

# Expected behaviour
I think the check should pass?

- this works in runtime under both under 3.9 and 3.10 python

  ```
  uv run --python=3.9 test_310_union.py 
  typing.Optional[str]
  $ uv run --python=3.10 test_310_union.py 
  str | None
  ```

- mypy is happy with it:
  ```
  $ uvx mypy test_310_union.py 
  Success: no issues found in 1 source file
  ```
  (I also tried `mypy --python-version=3.9` just in case, this also passes)
  
- ty works if I set `requires-python = ">=3.10"` in pyproject.toml


# Possibly relevant issues

- https://github.com/astral-sh/ty/issues/449
- the check was implemented here https://github.com/astral-sh/ruff/pull/18192
- I wasn't sure whether it's relevant to type aliases somehow (https://github.com/astral-sh/ty/issues/221) , but considering this works with python3.10 target, I don't think so

### Version

ty 0.0.1-alpha.6

---

_Renamed from "False positive for "PEP 604 union syntax is only available in Python 3.10 and later" when `requires-python = ">=3.9"`" to "False positive for `PEP 604 union syntax is only available in Python 3.10 and later` when `requires-python = ">=3.9"`" by @karlicoss on 2025-05-26 00:51_

---

_Label `bug` added by @AlexWaygood on 2025-05-26 06:23_

---

_Renamed from "False positive for `PEP 604 union syntax is only available in Python 3.10 and later` when `requires-python = ">=3.9"`" to "False positive for *PEP 604 union syntax is only available in Python 3.10 and later* when `requires-python = ">=3.9"`" by @MichaReiser on 2025-05-26 08:44_

---

_Renamed from "False positive for *PEP 604 union syntax is only available in Python 3.10 and later* when `requires-python = ">=3.9"`" to "False positive for `PEP 604 union syntax is only available in Python 3.10 and later` when `requires-python = ">=3.9"`" by @MichaReiser on 2025-05-26 08:45_

---

_Comment by @sharkdp on 2025-05-26 11:41_

Thank you for reporting this!

We *do* correctly handle `sys.version_info >= (3, 10)`, and we detect the corresponding branch as unreachable (Note for ty developers: it's easy to check this by trying to access a undefined symbol in the branch, which will not lead to a diagnostic). The problem is that we currently only silence a subset of all diagnostics in unreachable sections of code.

I will close this as a duplicate of #127, but add your example to the linked ticket.

---

_Closed by @sharkdp on 2025-05-26 11:41_

---

_Closed by @sharkdp on 2025-05-26 11:41_

---

_Label `unreachable-code` added by @sharkdp on 2025-05-26 11:42_

---
