```yaml
number: 2852
title: "`TCH004`: False positive caused by regression in v0.0.246"
type: issue
state: closed
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2023-02-13T11:41:02Z
updated_at: 2023-02-13T15:26:36Z
url: https://github.com/astral-sh/ruff/issues/2852
synced_at: 2026-01-10T11:09:45Z
```

# `TCH004`: False positive caused by regression in v0.0.246

---

_Issue opened by @ngnpope on 2023-02-13 11:41_

For the following code:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from typing import Final, Literal, TypeAlias

    RatingKey: TypeAlias = Literal["good", "fair", "poor"]

RATING_KEYS: Final[tuple[RatingKey, ...]] = ("good", "fair", "poor")
```

I get the following:

```console
❯ pipx inject ruff ruff==0.0.245
  injected package ruff into venv ruff
done! ✨ 🌟 ✨

❯ ruff --version
ruff 0.0.245

❯ ruff --isolated --select TCH004 bug.py 

❯ pipx inject ruff ruff==0.0.246
  injected package ruff into venv ruff
done! ✨ 🌟 ✨

❯ ruff --version
ruff 0.0.246

❯ ruff --isolated --select TCH004 bug.py 
bug.py:6:31: TCH004 Move import `typing.Literal` out of type-checking block. Import is used for more than type hinting.
Found 1 error.
```

I suspect this is related to #2774 and #2779. 

---

_Comment by @charliermarsh on 2023-02-13 13:57_

Thank you, will fix.

---

_Label `bug` added by @charliermarsh on 2023-02-13 13:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-13 14:40_

---

_Closed by @charliermarsh on 2023-02-13 15:26_

---
