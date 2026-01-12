```yaml
number: 999
title: F401 false positive with future annotations
type: issue
state: closed
author: joouha
labels:
  - bug
assignees: []
created_at: 2022-12-02T18:02:41Z
updated_at: 2022-12-02T18:23:25Z
url: https://github.com/astral-sh/ruff/issues/999
synced_at: 2026-01-12T15:54:40Z
```

# F401 false positive with future annotations

---

_@joouha_

Not sure if this is already fixed in #992, but given the follow code, ruff incorrectly flags F401:

```python
from __future__ import annotations

from typing import Callable, Optional


def my_func(param: "Optional[Callable]" = None) -> "None":
    pass
```

```
Found 1 error(s).
example.py:5:20: F401 `typing.Callable` imported but unused
1 potentially fixable with the --fix option.
```

If `from __future__ import annotations` is not used, there are no errors.

This happens with Python 3.8, 3.9, 3.10, 3.11, and ruff 0.0.151

P.S. Thanks for making ruff so fast

---

_Label `bug` added by @charliermarsh on 2022-12-02 18:06_

---

_Comment by @charliermarsh on 2022-12-02 18:06_

Cool, still seeing this on `main` so will fix!

---

_Comment by @charliermarsh on 2022-12-02 18:06_

(Although if you have `from __future__ import annotations` enabled, I think you can safely remove the quotes around the annotation?)

---

_Closed by @charliermarsh on 2022-12-02 18:22_

---

_Comment by @charliermarsh on 2022-12-02 18:23_

This is going out in [v0.0.152](https://github.com/charliermarsh/ruff/releases/tag/v0.0.152) (building now).

---
