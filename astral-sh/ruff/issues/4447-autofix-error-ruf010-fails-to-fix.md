---
number: 4447
title: "[Autofix error]: `RUF010` fails to fix"
type: issue
state: closed
author: Saransh-cpp
labels: []
assignees: []
created_at: 2023-05-16T08:34:43Z
updated_at: 2023-05-16T12:23:43Z
url: https://github.com/astral-sh/ruff/issues/4447
synced_at: 2026-01-10T01:22:43Z
---

# [Autofix error]: `RUF010` fails to fix

---

_Issue opened by @Saransh-cpp on 2023-05-16 08:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Ruff version
`v0.0.267`

## Failing PR
https://github.com/scikit-hep/vector/pull/345

## Error 
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `src/vector/_methods.py`, the rule codes RUF010, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

src/vector/_methods.py:3260:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3260:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3269:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3269:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3284:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3284:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3492:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3492:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3501:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3501:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3516:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3516:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3753:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3753:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3762:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3762:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3777:20: RUF010 Use conversion in f-string
src/vector/_methods.py:3777:37: RUF010 Use conversion in f-string
src/vector/_methods.py:3945:28: RUF010 Use conversion in f-string
src/vector/_methods.py:3958:28: RUF010 Use conversion in f-string
src/vector/_methods.py:3960:28: RUF010 Use conversion in f-string
src/vector/_methods.py:4119:25: RUF010 Use conversion in f-string
Found 22 errors.
```
## Relevant code lines
```py
raise TypeError(
    f"{repr(self)} and {repr(other)} do not have the same dimension"    # line 3260, 3269, 3284, 3492, 3501, 3516, 3753, 3762, 3777
)
```
```py
raise TypeError(f"{repr(v)} is not a vector.Vector")    # line 3945
```
```py
raise TypeError(f"{repr(one)} is not a Vector")    # line 3958, 3960
```
```py
raise TypeError(
    f"function {repr('.'.join(name.split('.')[-2:]))} has no signature {signature}"    # line 4119
)
```

## pyproject.toml config
```toml
[tool.ruff]
select = [
  "E", "F", "W", # flake8
  "B", "B904",   # flake8-bugbear
  "I",           # isort
  "C4",          # flake8-comprehensions
  "ISC",         # flake8-implicit-str-concat
  "PGH",         # pygrep-hooks
  "PIE",         # flake8-pie
  "PL",          # pylint
  "PT",          # flake8-pytest-style
  "RUF",         # Ruff-specific
  "SIM",         # flake8-simplify
  "T20",         # flake8-print
  "UP",          # pyupgrade
  "YTT",         # flake8-2020
]
extend-ignore = [
  "PLR",   # Design related pylint codes
  "E501",  # Line too long
]
target-version = "py37"
typing-modules = ["vector._typeutils"]
src = ["src"]
unfixable = [
  "T20",  # Removes print statements
  "F841", # Removes unused variables
]
exclude = []
isort.required-imports = ["from __future__ import annotations"]

[tool.ruff.per-file-ignores]
"noxfile.py" = ["T20"]
"tests/*" = ["T20"]
"src/vector/backends/_numba_object.py" = ["PGH003"]
"tests/backends/test_operators.py" = ["SIM201", "SIM202"]
```

## pre-commit config
```yaml
- repo: https://github.com/charliermarsh/ruff-pre-commit
  rev: "v0.0.267"
  hooks:
    - id: ruff
      args: ["--fix", "--show-fixes"]
```

## MWE

Weirdly, this MWE gives no errors -
```py
class RuffRepr:
    def __init__(self ,x) -> None:
        self.x = x

    def __repr__(self) -> str:
        return "RuffRepr"

    def equate_it(self, other):
        raise TypeError(
            f"{repr(self)} and {repr(other)} do not have the same dimension"
        )


class Other:
    def __init__(self, x) -> None:
        self.x = x

    def __repr__(self) -> str:
        return "Other"
    
other = Other(3)
ruff_repr = RuffRepr(3)

ruff_repr.equate_it(other)
```

```
ruff.....................................................................Failed
- hook id: ruff
- files were modified by this hook

Fixed 5 errors:
- testit.py:
    2 × RUF010 (explicit-f-string-type-conversion)
    1 × I002 (missing-required-import)
    1 × W293 (blank-line-with-whitespace)
    1 × I001 (unsorted-imports)

Found 5 errors (5 fixed, 0 remaining).
```

```py
from __future__ import annotations


class RuffRepr:
    def __init__(self ,x) -> None:
        self.x = x

    def __repr__(self) -> str:
        return "RuffRepr"

    def equate_it(self, other):
        raise TypeError(
            f"{self!r} and {other!r} do not have the same dimension"
        )


class Other:
    def __init__(self, x) -> None:
        self.x = x

    def __repr__(self) -> str:
        return "Other"

other = Other(3)
ruff_repr = RuffRepr(3)

ruff_repr.equate_it(other)
```

---

_Comment by @JonathanPlasse on 2023-05-16 10:46_

- Should be fixed by #4423

---

_Comment by @Saransh-cpp on 2023-05-16 12:23_

Thanks! Will close this!

---

_Closed by @Saransh-cpp on 2023-05-16 12:23_

---

_Referenced in [Flexget/Flexget#3769](../../Flexget/Flexget/pulls/3769.md) on 2023-05-16 15:13_

---
