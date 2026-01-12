```yaml
number: 6372
title: "Add autofix for import convention's `banned-from` rule"
type: issue
state: open
author: divaltor
labels:
  - fixes
  - accepted
assignees: []
created_at: 2023-08-06T11:35:47Z
updated_at: 2025-02-08T18:04:47Z
url: https://github.com/astral-sh/ruff/issues/6372
synced_at: 2026-01-12T15:54:46Z
```

# Add autofix for import convention's `banned-from` rule

---

_@divaltor_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
With enabled TCH and ICN rules there is some, I guess, expected behavior. But is there any possible way to implement auto-fix for this case?

pyproject.toml
```
[tool.poetry]
name = "ruff-test"
version = "0.1.0"
description = ""
authors = ["Test <test@example.com>"]
readme = "README.md"
packages = [{include = "ruff_test"}]

[tool.poetry.dependencies]
python = "^3.11"
pandas = "^2.0.3"


[tool.poetry.group.dev.dependencies]
ruff = "^0.0.282"

[tool.ruff]
extend-select = ["TCH", "ICN"]
target-version = "py311"


[tool.ruff.flake8-import-conventions]
banned-from = ["typing"]
[tool.ruff.flake8-import-conventions.extend-aliases]
"typing" = "t"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

main.py
```python
from __future__ import annotations

import pandas as pd


def main(df: pd.DataFrame) -> int:
    return len(df)
```

Command
```shell
$ ruff . --fix --show-fixes
main.py:2:1: ICN003 Members of `typing` should not be imported explicitly

Fixed 1 error:
- main.py:
    1 × TCH002 (typing-only-third-party-import)

Found 2 errors (1 fixed, 1 remaining)
```

Actual behavior:
```python
from __future__ import annotations
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    import pandas as pd


def main(df: pd.DataFrame) -> int:
    return len(df)
```

Wanted (if possible) behavior:
```python
from __future__ import annotations
import typing as t

if t.TYPE_CHECKING:
    import pandas as pd


def main(df: pd.DataFrame) -> int:
    return len(df)
```

Is that hard to implement autofix to such import with specified alias in config?

---

_Renamed from "Incompatible TCH and ICN rules" to "Improvement for using TCH and ICN rules together" by @divaltor on 2023-08-06 11:37_

---

_Comment by @charliermarsh on 2023-08-06 13:27_

I think we can support renaming the typing import there. I actually thought we supported it already, I‘ll look into it :)

---

_Comment by @charliermarsh on 2023-08-06 14:16_

Oh I see, the goal is to autofix the `from typing import ...` to `import typing as t`. We don't yet support that, but we could.

---

_Label `autofix` added by @charliermarsh on 2023-08-06 14:16_

---

_Label `accepted` added by @charliermarsh on 2023-08-06 14:16_

---

_Renamed from "Improvement for using TCH and ICN rules together" to "Add autofix for import convention's `banned-from` rule" by @charliermarsh on 2023-08-06 14:16_

---

_Comment by @tjkuson on 2024-05-30 18:00_

I'm going to try adding an autofix for this rule this weekend

---

_Comment by @tjkuson on 2024-06-17 21:31_

I've stopped working on this. I might return to it in the future. It's a bit tricky: it needs to be both deferred and undeferred and it needs to resolve which import pattern to use instead whilst respecting present imports. The edge cases are tough.

---
