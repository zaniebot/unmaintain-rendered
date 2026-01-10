```yaml
number: 12513
title: "F401 creates infinite loop in preview mode when autofixing an unused first-party submodule import in an `__init__.py` file that has an `__all__` definition"
type: issue
state: closed
author: rshanker779
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2024-07-25T15:39:36Z
updated_at: 2024-07-31T12:34:31Z
url: https://github.com/astral-sh/ruff/issues/12513
synced_at: 2026-01-10T11:09:54Z
```

# F401 creates infinite loop in preview mode when autofixing an unused first-party submodule import in an `__init__.py` file that has an `__all__` definition

---

_Issue opened by @rshanker779 on 2024-07-25 15:39_

When having an `__init__` file with an `__all__` and an import that does not appear in `__all__`, an inifinte loop occurs as ruff tries to add the import to `__all__`. Full recreation details are below



List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.  
- `__all__` init bug
A minimal code snippet that reproduces the bug.
```python
import custom_module.a
from custom_module import b

__all__ = ['b']
```

This is with module structure
```
src/
   - custom_module
     - __init__.py # with the contents above
       - a
         - __init__.py #empty
       - b 
         -  __init__.py #empty
```

Running 
```shell
ruff check --select=I,F401 --fix src/custom_module
```

Causes ruff to repeatedly add `custom_module.a` to the `__all__` until it errors

The final output is
```python
import custom_module.a
from custom_module import b

__all__ = ['b', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a', 'custom_module.a']
```



Ruff settings

```toml
[tool.ruff]
target-version = "py310"

line-length = 120
format.preview = true
format.docstring-code-format = true
lint.select = [
  "A",      # flake8-builtins
  "AIR",
  "ASYNC",
  "ASYNC1",
  "B",      # flake8-bugbear
  "BLE",    # flake8-blind-except
  "C4",
  "D",
  "DJ",
  "DTZ",
  "E",      # pycodestyle
  "ERA",    # eradicate
  "EXE",
  "F",      # pyflakes
  "FA",
  "FLY",    # flynt
  "FURB",
  "G",      # flake8-logging-format
  "I",      # isort
  "ICN",
  "INP",    # flake8-no-pep420
  "INT",
  "LOG",
  "N",      #pep8-naming
  "NPY",    # numpy
  "PD",     #pandas
  "PERF",
  "PGH",    # pygrep-hooks
  "PIE",
  "PLE",
  "PLW",
  "PT",     # flake8-pytest-style
  "PTH",    # flake8-use-pathlib
  "PYI",
  "Q",
  "RET",
  "RSE",
  "RUF",
  "S",      # flake8-bandit
  "SIM",
  "SLOT",
  "T",
  "TID",
  "TRY",    # tryceratops
  "UP",
  "W",      # pycodestyle
  "YTT",
]
lint.ignore = [
  "D100",  
  "D103",
  "D104",
  "D105",
  "D106",
  "D401",   
  "E501",
  "PIE790",
  "PLW1514",
  "PT001",
  "RUF002",
  "S101",
  "SIM300",
  "TRY003",
  "TRY300",
  "TRY301",
  # https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules
  "W191", # Enforced by the formatter
]
lint.per-file-ignores."**/{tests,scripts}/*" = [
  "ARG",
  "D",
  "INP",
  "PLR",
  "S101",
  "T20",
]
lint.fixable = [
  "F401",
  "I",
]
lint.pep8-naming.classmethod-decorators = [
  "pydantic.validator",
]
lint.pydocstyle.convention = "numpy"
lint.preview = true
```

Ruff version
```
ruff 0.5.2
```

---

_Label `bug` added by @charliermarsh on 2024-07-25 15:41_

---

_Label `fixes` added by @charliermarsh on 2024-07-25 15:41_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-07-26 08:04_

---

_Comment by @jack-mcivor on 2024-07-26 08:52_

I can reproduce this on v0.5.5 with
```shell
ruff check --isolated --preview --select=F401 --fixable=F401 --fix src/custom_module
``` 

---

_Comment by @AlexWaygood on 2024-07-29 13:59_

I think the issue is that the F401 fix is trying to add `"custom_module.a"` to `__all__` for the autofix -- but it shouldn't be, because `"custom_module.a"` isn't a valid identifier. That means that when it comes around on the next loop, it finds that there the `custom_module.a` binding created by the import still isn't used at all (normally including it in `__all__` would mean that it's now counted as used, but here it doesn't because the inclusion in `__all__` is invalid). So it again tries to add `"custom_module.a"` to `__all__` to fix it not being used. And so on and so on, _ad infinitum_.

---

_Renamed from "[Infinite loop] F401 and RUF022 causing a loop in `__all__`" to "F401 fix causes an infinite loop when an unused first-party import in an `__init__.py` file isn't a valid identifier" by @AlexWaygood on 2024-07-29 14:00_

---

_Renamed from "F401 fix causes an infinite loop when an unused first-party import in an `__init__.py` file isn't a valid identifier" to "F401 creates infinite loop when autofixing an unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" by @AlexWaygood on 2024-07-29 16:46_

---

_Label `preview` added by @AlexWaygood on 2024-07-29 16:46_

---

_Renamed from "F401 creates infinite loop when autofixing an unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" to "F401 creates infinite loop in preview mode when autofixing an unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" by @AlexWaygood on 2024-07-29 16:46_

---

_Closed by @AlexWaygood on 2024-07-31 12:34_

---
