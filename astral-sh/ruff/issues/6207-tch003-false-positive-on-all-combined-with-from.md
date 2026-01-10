```yaml
number: 6207
title: "`TCH003` false positive on `__all__` combined with `from __future__ import annotations`"
type: issue
state: closed
author: mkniewallner
labels:
  - bug
assignees: []
created_at: 2023-07-31T19:25:16Z
updated_at: 2023-08-01T15:04:47Z
url: https://github.com/astral-sh/ruff/issues/6207
synced_at: 2026-01-10T11:09:48Z
```

# `TCH003` false positive on `__all__` combined with `from __future__ import annotations`

---

_Issue opened by @mkniewallner on 2023-07-31 19:25_

[`TCH003`](https://beta.ruff.rs/docs/rules/typing-only-standard-library-import/) seems to incorrectly trigger when re-exporting a module in `__all__` if using `from __future__ import annotations`.

Minimal reproducible code:
```python
# foo.py
from __future__ import annotations

from logging import getLogger

__all__ = ("getLogger",)


def foo() -> None:
    pass
```

```shell
$ pipx run ruff==0.0.281 foo.py
foo.py:3:21: TCH003 [*] Move standard library import `logging.getLogger` into a type-checking block
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

In the example above, we import `getLogger` from `logging` to re-export it in `__all__`, and `TCH003` will get triggered, although it should not.

Note that this does not trigger when removing `from __future__ import annotations`. It also doesn't trigger in version 0.0.280, so this seems to be related to a change in 0.0.281.

What's even weirder is that the violation will also not trigger in the example if we remove the return type (`-> None`) from the function.

Command used:
```shell
pipx run ruff==0.0.281 foo.py
```

Minimal Ruff settings to reproduce:
```toml
[tool.ruff]
select = [
    # flake8-type-checking
    "TCH",
]
```

Ruff version:
```
ruff 0.0.281
```

---

_Comment by @charliermarsh on 2023-07-31 19:25_

Very weird, taking a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-31 19:26_

---

_Label `bug` added by @charliermarsh on 2023-07-31 19:27_

---

_Comment by @charliermarsh on 2023-07-31 19:30_

This is a subtle bug related to the order in which we visit things internally. I'll probably push a hotfix for this today.

---

_Closed by @charliermarsh on 2023-07-31 19:46_

---

_Comment by @charliermarsh on 2023-08-01 15:04_

This should be fixed in v0.0.282, which is out now.

---
