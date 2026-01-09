---
number: 4020
title: TCH003 false positive when typing nested functions
type: issue
state: closed
author: VoodaGod
labels:
  - bug
assignees: []
created_at: 2023-04-19T13:33:26Z
updated_at: 2023-04-19T18:43:56Z
url: https://github.com/astral-sh/ruff/issues/4020
synced_at: 2026-01-07T13:12:14-06:00
---

# TCH003 false positive when typing nested functions

---

_Issue opened by @VoodaGod on 2023-04-19 13:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

consider `test.py`
```
from collections.abc import Callable, Collection


def test():
    def nested() -> Callable[[Collection[str]], list[str]]:
        def double_nested(x: Collection[str]) -> list[str]:
            return list(x)

        return double_nested

    a = nested()
    return a(("a", "b"))


print(test())

```
outputs
```
['a', 'b']
```

ruff complains:
```
test.py:1:29: TCH003 Move standard library import `collections.abc.Callable` into a type-checking block  
test.py:1:39: TCH003 Move standard library import `collections.abc.Collection` into a type-checking block
```

appeasing ruff
```
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable, Collection


def test():
    def nested() -> Callable[[Collection[str]], list[str]]:
        def double_nested(x: Collection[str]) -> list[str]:
            return list(x)

        return double_nested

    a = nested()
    return a(("a", "b"))


print(test())

```
breaks the code:
```
Traceback (most recent call last):
  File "c:\Programs\PCGUserdir\Repos\SmartSensorBus_configurator\tempCodeRunnerFile.py", line 18, in <module>
    print(test())
          ^^^^^^
  File "c:\Programs\PCGUserdir\Repos\SmartSensorBus_configurator\tempCodeRunnerFile.py", line 8, in test
    def nested() -> Callable[[Collection[str]], list[str]]:
                    ^^^^^^^^
NameError: name 'Callable' is not defined. Did you mean: 'callable'?
```


the command i used to invoke ruff: `python -m ruff .`
my pyproject.toml ruff settings:
```
[tool.ruff]
line-length = 100
ignore-init-module-imports = true
extend-select = [
	"W",      # pydocstyle warnings
	"C90",    # mccabe complexity
	"I",      # import sorting
	"N",      # pep8 naming
	"UP",     # old syntax/constructs
	"YTT",    # old syntax/constructs
	"B",      # common sources of bugs
	"A",      # shadowing builtins
	"COM",    # trailing commas
	"C4",     # comprehensions
	"EXE",    # shebangs / executable .py files
	"ISC",    # implicit string concatenation
	"PT",     # pytest
	"Q",      # quotes
	"RET502", # missing return
	"RET503", # missing return
	"SIM",    # simplify code
	"TCH",    # type checking imports
	"ARG",    # unused arguments
	"PTH",    # use pathlib
	"PLE",    # pylint errors
]
ignore = [
	"E501",   # let black handle line length
	"COM812", # let black handle missing trailing commas
]
unfixable = [
	"F841", # unused variable
	"F401", # unused import
]
```

ruff version: `ruff 0.0.261`
python version: `Python 3.11.2`

---

_Label `bug` added by @charliermarsh on 2023-04-19 13:35_

---

_Comment by @charliermarsh on 2023-04-19 16:20_

Oh interesting, I thought annotations for functions within functions weren't required to be available at runtime, but I must've misunderstood. Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-19 18:20_

---

_Referenced in [astral-sh/ruff#4028](../../astral-sh/ruff/pulls/4028.md) on 2023-04-19 18:24_

---

_Closed by @charliermarsh on 2023-04-19 18:43_

---
