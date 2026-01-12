```yaml
number: 3860
title: "flake8-pyi: PYI015 false positive on TypeVar assignment"
type: issue
state: closed
author: bluetech
labels: []
assignees: []
created_at: 2023-04-03T10:17:17Z
updated_at: 2023-04-03T15:28:48Z
url: https://github.com/astral-sh/ruff/issues/3860
synced_at: 2026-01-12T15:54:44Z
```

# flake8-pyi: PYI015 false positive on TypeVar assignment

---

_@bluetech_

Given this pyi file:

```py
from typing import TypeVar
_T = TypeVar('_T')
```

running

```
ruff --select PYI015 --isolated --no-cache x.pyi
```

gives

```
x.pyi:2:6: PYI015 [*] Only simple default values allowed for assignments
```

I think `TypeVar` assignments should be excluded from this rule as they appear naturally in pyi files.

Possible workaround is `_T: TypeVar = TypeVar('_T')`, however that's unnecessarily redundant, and pyright doesn't like it.

Similar cases - `TypeAlias` (already handled), `NewType`, `TypeVarTuple`, `ParamSpec`

From a quick test, this doesn't happen on flake8-pyi.

I think I can provide a PR for this.

ruff 0.0.260

---

_Closed by @charliermarsh on 2023-04-03 15:28_

---
