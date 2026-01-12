```yaml
number: 7816
title: False positive hardcoded-password-string (S105)
type: issue
state: closed
author: rossnomann
labels:
  - needs-decision
assignees: []
created_at: 2023-10-04T17:01:04Z
updated_at: 2023-11-23T13:32:21Z
url: https://github.com/astral-sh/ruff/issues/7816
synced_at: 2026-01-12T15:54:47Z
```

# False positive hardcoded-password-string (S105)

---

_@rossnomann_

A minimal code snippet that reproduces the bug:


```python
import enum


class Method(str, enum.Enum):
    token_get = "token_get"
    token_refresh = "token_refresh"

```

The current Ruff settings (`pyproject.toml`):
```toml
[tool.ruff]
line-length = 120
ignore = [
  "ANN101",  # missing-type-self
  "ANN102",  # missing-type-cls
  "D",  # pydocstyle
  "FIX",  #  flake8-fixme
  "TD003"  # Missing issue link
]
select = ["ALL"]
target-version = "py311"
```
Ruff version: 0.0.292

---

_Label `needs-decision` added by @charliermarsh on 2023-10-04 17:11_

---

_Comment by @charliermarsh on 2023-10-05 15:15_

Bandit flags these too. Could you instead use `enum.auto()` for the values? IIUC that has the same effect, and avoids the hardcoded constant.

---

_Comment by @rossnomann on 2023-10-05 16:31_

Yes, `enum.auto` works well, thanks

---

_Comment by @charliermarsh on 2023-10-05 16:32_

I'm gonna close as "working as intended". Thanks for filing!

---

_Closed by @charliermarsh on 2023-10-05 16:32_

---

_Comment by @ashrub-holvi on 2023-11-23 13:32_

discussion continues [here](https://github.com/astral-sh/ruff/issues/8789)

---
