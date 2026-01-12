```yaml
number: 3958
title: "`S101` should ignore `assert` statements in `if TYPE_CHECKING` blocks"
type: issue
state: closed
author: bryanforbes
labels: []
assignees: []
created_at: 2023-04-13T14:11:32Z
updated_at: 2023-04-13T18:20:46Z
url: https://github.com/astral-sh/ruff/issues/3958
synced_at: 2026-01-12T15:54:44Z
```

# `S101` should ignore `assert` statements in `if TYPE_CHECKING` blocks

---

_@bryanforbes_

Ruff version: 0.0.261

When writing type-checked code, it is sometimes necessary to let the type checker know about conditions it cannot be aware of using `if TYPE_CHECKING` blocks. Here's a (contrived) example:

```python
from __future__ import annotations

from typing import TYPE_CHECKING


class Thing:
    some_prop: str | None

    def _init(self) -> None:
        self.some_prop = 'set'

    def _internal_method(self, arg: str) -> None:
        print(arg)

    def start(self) -> None:
        self._init()

        if TYPE_CHECKING:
            assert self.some_prop is not None

        self._internal_method(self.some_prop)
```

There is no way for the type checker to know that `_init()` set `some_prop`, so it still thinks `some_prop` is `str | None`, so we have to help it out with an assert statement. Obviously, `if TYPE_CHECKING` blocks don't run at runtime, so the `assert` will never run. It would be nice if `ruff` ignored `assert` statements in `if TYPE_CHECKING` blocks for the `S101` rule.

---

_Closed by @charliermarsh on 2023-04-13 18:20_

---
