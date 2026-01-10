```yaml
number: 22223
title: TC002 conflicts with TC004
type: issue
state: closed
author: Samoed
labels:
  - question
assignees: []
created_at: 2025-12-27T11:56:03Z
updated_at: 2025-12-27T15:48:51Z
url: https://github.com/astral-sh/ruff/issues/22223
synced_at: 2026-01-10T11:10:00Z
```

# TC002 conflicts with TC004

---

_Issue opened by @Samoed on 2025-12-27 11:56_

### Summary

`TC002` conflicts with `TC004` when `flake8-type-checking` is activated. 

Example:
```py
from typing import TYPE_CHECKING

# will conflict with TC002
from typing_extensions import NotRequired, TypedDict

# will conflict with TC004
# from typing_extensions import TypedDict
# if TYPE_CHECKING:
#     from typing_extensions import NotRequired


class Example(TypedDict):
    a: int
    b: NotRequired[float]
```

```toml
[project]
name = "test-ruff"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "ruff>=0.14.10",
    "typing-extensions>=4.15.0",
]

[tool.ruff]
target-version = "py310"


[tool.ruff.lint]
select = [
    "TC",  # flake8-type-checking
]
typing-modules = ["typing_extensions"]
future-annotations = true

[tool.ruff.lint.flake8-type-checking]
strict = true
```

Playground example: https://play.ruff.rs/3665cc48-65b5-401d-bfff-20cadb9db6f4

### Version

ruff 0.14.10 (45bbb4cbf 2025-12-18)

---

_Comment by @ntBre on 2025-12-27 15:10_

Thanks for the report! 

I'm not seeing any conflict in your playground example. If I apply the suggested fix there are no diagnostics left. This is the output after the fix:

```py
from __future__ import annotations
from typing import TYPE_CHECKING

# will conflict with TC002
from typing_extensions import TypedDict

if TYPE_CHECKING:
    from typing_extensions import NotRequired

# will conflict with TC004
# from typing_extensions import TypedDict
# if TYPE_CHECKING:
#     from typing_extensions import NotRequired


class Example(TypedDict):
    a: int
    b: NotRequired[float]
```

Note the `__future__` import. If I disable the `future-annotations` setting, I don't see any diagnostic to begin with.

---

_Label `question` added by @ntBre on 2025-12-27 15:10_

---

_Comment by @Samoed on 2025-12-27 15:13_

Ah, yes, thanks. I forgot that and thought it would be added automatically. Feel free to close then.

UPD. It will add future with unsafe fixes

---

_Closed by @Samoed on 2025-12-27 15:48_

---
