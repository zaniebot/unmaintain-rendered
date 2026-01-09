---
number: 9985
title: "`unsorted-imports` (`I001`) - does not detect duplicated imports when one of the imports is inside a `TYPE_CHECKING` block"
type: issue
state: open
author: DetachHead
labels: []
assignees: []
created_at: 2024-02-14T13:39:57Z
updated_at: 2024-02-14T13:39:57Z
url: https://github.com/astral-sh/ruff/issues/9985
synced_at: 2026-01-07T13:12:15-06:00
---

# `unsorted-imports` (`I001`) - does not detect duplicated imports when one of the imports is inside a `TYPE_CHECKING` block

---

_Issue opened by @DetachHead on 2024-02-14 13:39_

```py
from collections.abc import Iterable
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Iterable  # no error
```
[playground](https://play.ruff.rs/8a663307-3871-4671-8ca1-3b7166634195)

usually if there are duplicated imports, `I001` detects it:
```py
from collections.abc import Iterable
from collections.abc import Iterable  # error: Import block is un-sorted or un-formatted
```

---
