---
number: 20884
title: Ruff ruleset D ignores protected python files.
type: issue
state: closed
author: Jtachan
labels: []
assignees: []
created_at: 2025-10-15T07:45:49Z
updated_at: 2025-10-16T13:37:05Z
url: https://github.com/astral-sh/ruff/issues/20884
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff ruleset D ignores protected python files.

---

_Issue opened by @Jtachan on 2025-10-15 07:45_

### Summary

**Summary**
The command `ruff check` seems to ignore all D rules at protected python modules as `_my_module.py`. 
I have not been able to find within the documentation any rule that specifies that this is a feature, so I assume it is a bug.

**Requested solution**
Add a new parameter for pydocstyle: `ignore-protected-modules = false` allowing any user of ruff to decide whether to ignore the D rules at protected modules.

**Complete description**
I realized that I had a protected module with a method without a docstring, and ruff was not printing any warning.

The **playground** does not reproduce the issue, but I was able to reproduce it with the following file:
```python
# my_class.py

class MyClass:
    def __init__(self, data: float):
        self.data = [data]

    def add_new_data(self, value):  # Missing type of 'value' on purpose to get always some lint warning
        self.data.append(value)
```

Here the linter printed the following warnings:
```
my_class.py:1:1: D100 Missing docstring in public module
my_class.py:2:7: D101 Missing docstring in public class
my_class.py:6:9: D102 Missing docstring in public method
my_class.py:6:28: ANN001 Missing type annotation for function argument `value`
Found 4 errors.
```

However, when the module is renamed to `_my_class.py`, only the following messages are printed:
```
_my_class.py:6:28: ANN001 Missing type annotation for function argument `value`
Found 1 error.
```

**ruff.toml**
```
output-format = "concise"

[lint]
select = [
    "A",    # flake8-builtins
    "ANN",  # flake8-annotations
    "ARG",  # flake8-unused-arguments
    "B",    # flake8-bugbear
    "BLE",  # flake8-blind-except
    "C4",   # flake8-comprehensions
    "D",    # pydocstyle
    "E",    # pycodestyle error
    "F",    # Pyflakes
    "FA",   # flake8-future-annotations
    "I",    # isort
    "ICN",  # flake8-import-conventions
    "ISC",  # flake8-implicit-str-concat
    "N",    # pep8-naming
    "NPY",  # NumPy-specific rules
    "PD",   # pandas-vet
    "PIE",  # flake8-pie
    "PL",   # pylint
    "PT",   # flakeru8-pytest
    "RET",  # flake8-return
    "RUF",  # Ruff-specific rules
    "SIM",  # flake8-simplify
    "TC",   # flake8-type-checking
    "W",    # pycodestyle warning
]
ignore = [
    "ANN002",   # missing type for *args
    "ANN003",   # missing type for **kwargs
    "D401",     # first line in docstring as imperative mood
    "PLR6301",  # no-self-use
    "PLR0913",  # too-many-arguments
    "PLR0904",  # too-many-public-methods
    "PLR0916",  # too-many-boolean-expressions
    "PLR0912",  # too-many-branches
    "PLR0904",  # too-many-public-methods
    "PLR0911",  # too-many-return-statements
    "PLR0914",  # too-many-local-variables
    "PLR0915",  # too-many-statements
    "PLR0917",  # too-many-positional-arguments
    "PLC1901",  # compare-to-empty-string
    "PLR2004",  # magic-value-comparison
]
preview = true

[lint.flake8-annotations]
allow-star-arg-any = true
mypy-init-return = true
suppress-none-returning = true

[lint.flake8-unused-arguments]
ignore-variadic-names = true

[lint.isort]
split-on-trailing-comma=false

[lint.pycodestyle]
ignore-overlong-task-comments = true
```

### Version

0.14.0

---

_Comment by @ntBre on 2025-10-16 13:37_

It looks like this is a duplicate of https://github.com/astral-sh/ruff/issues/9946, but I think it makes sense to have a configuration option here.

---

_Closed by @ntBre on 2025-10-16 13:37_

---
