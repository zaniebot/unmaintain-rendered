```yaml
number: 4222
title: "Pandas vet incorrectly marks `torch` code as breaking the pandas linting rules"
type: issue
state: closed
author: pawel-biernacki-roche
labels:
  - bug
assignees: []
created_at: 2023-05-04T12:11:49Z
updated_at: 2023-05-10T14:04:29Z
url: https://github.com/astral-sh/ruff/issues/4222
synced_at: 2026-01-10T11:09:47Z
```

# Pandas vet incorrectly marks `torch` code as breaking the pandas linting rules

---

_Issue opened by @pawel-biernacki-roche on 2023-05-04 12:11_

**Issue**:
Pandas vet incorrectly marks `torch` code as breaking the pandas linting rules

**Current result**:
```path/to/file.py:61:35: PD002 `inplace=True` should be avoided; it has inconsistent behavior```

**Expected result**:
No issues raised

**Torch docs**: https://pytorch.org/docs/stable/generated/torch.nn.ReLU.html

**Minimal code/context**:
```!python
    def __init__(
        self,
        <abbreviated>
    ):
        super().__init__()
        <abbreviated>
        self.relu = torch.nn.ReLU(inplace=True)
```
**Command**:
`ruff path/to/file.py`

**Settings**:
```!toml
[tool.ruff]
select = [
    "B", # bugbear
    "C90", # mccabe
    "E", # pycodestyle error
    "F", # pyflakes
    "PD", # pandas vet
    "PL", # pylint
    "RUF", # ruff specific
    "UP", # pyupgrade
    "W", # pycodestyle warning
]
ignore = [
    "B006", # do not use mutable data structures for argument defaults
    "B008", # do not perform function call in argument defaults
    "E731", # lambda-assignment
    "PD901", # bad variable name
    "PLR0911", # too many return statements
    "PLR0912", # too many branches
    "PLR0913", # too many arguments to function call
    "PLR0915", # too many statements
    "PLR2004", # magic numbers
    "PLW2901", # loop variable overwritten by assignment target
]
line-length = 80
target-version = "py37"
```

**Version**:
`ruff 0.0.257`



---

_Label `bug` added by @charliermarsh on 2023-05-04 13:15_

---

_Comment by @evanrittenhouse on 2023-05-04 14:06_

I wonder if/how we could check this given our current state. 

Currently we just check if [`inplace` is used as a kwarg](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs#L66). To avoid false positives, I think we'd have to store all variables that reference DataFrames/Series and apply that check to methods on those variables only. Given that we don't have type-checking (yet) it may be hard to do that in a way that stays up-to-date with Pandas.

---

_Comment by @charliermarsh on 2023-05-05 18:10_

Could we check if it's a call to a `torch` function? That will always be a false positive.

---

_Comment by @charliermarsh on 2023-05-05 18:10_

I guess, more generally, we could check if it's a call to any imported function that isn't `pandas`.

---

_Comment by @evanrittenhouse on 2023-05-06 19:37_

Sure! I can pick this up

---

_Closed by @MichaReiser on 2023-05-10 14:04_

---
