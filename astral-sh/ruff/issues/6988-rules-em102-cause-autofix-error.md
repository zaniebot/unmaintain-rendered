```yaml
number: 6988
title: Rules EM102 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-29T18:18:18Z
updated_at: 2023-11-09T05:22:17Z
url: https://github.com/astral-sh/ruff/issues/6988
synced_at: 2026-01-10T11:09:49Z
```

# Rules EM102 cause autofix error

---

_Issue opened by @qarmin on 2023-08-29 18:18_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select EM102 --no-cache --fix
```

file content:
```
class Dependency:
    def validate(self) -> bool:
                raise ValueError(f"""Must provide a value for argument '{self.argument}' if {self.mode.__name__} of: {", ".join(f"'{arg}'" for arg in self.arguments)} are truthy.""")
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `iohandler8090.py`, the rule codes EM102, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

iohandler8090.py:1:1: D100 Missing docstring in public module
iohandler8090.py:1:1: CPY001 Missing copyright notice at top of file
iohandler8090.py:1:7: D101 Missing docstring in public class
iohandler8090.py:2:9: D102 Missing docstring in public method
iohandler8090.py:2:18: ANN101 Missing type annotation for `self` in method
iohandler8090.py:3:1: E117 Over-indented
iohandler8090.py:3:23: TRY003 Avoid specifying long messages outside the exception class
iohandler8090.py:3:34: EM102 Exception must not use an f-string literal, assign to variable first
iohandler8090.py:3:89: E501 Line too long (182 > 88 characters)
```

[iohandler8090.py.zip](https://github.com/astral-sh/ruff/files/12467943/iohandler8090.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-30 00:43_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-30 00:43_

---

_Closed by @dhruvmanila on 2023-11-09 05:22_

---
