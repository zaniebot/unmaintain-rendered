```yaml
number: 2634
title: "Not treating `@staticmethod`s as static within an `abc.ABCMeta` (sub)class"
type: issue
state: closed
author: trevenrawr
labels:
  - bug
assignees: []
created_at: 2023-02-07T19:38:58Z
updated_at: 2023-02-07T19:57:05Z
url: https://github.com/astral-sh/ruff/issues/2634
synced_at: 2026-01-10T11:09:45Z
```

# Not treating `@staticmethod`s as static within an `abc.ABCMeta` (sub)class

---

_Issue opened by @trevenrawr on 2023-02-07 19:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

While running `ruff 0.0.243` on some code, I had a confusing linting output, which can be reproduced with the following snippet:

```python
from abc import ABCMeta


class MetaClass(ABCMeta):
    @staticmethod
    def i_dont_get_cls_param(not_cls) -> bool:
        return False
```

Despite the method being noted as static, `ruff` is complaining that it should take a `cls` parameter:
```
$ ruff ruff.py
ruff.py:6:30: N804 First argument of a class method should be named `cls`
Found 1 error.
```

Based on the usage of this function in other code, it is indeed static, so this seems like it's some sort of precedence issue between treating all methods of `ABCMeta` as classmethods and the `@staticmethod` decorator.

P.S. Thanks for such a blazingly fast linter and sorter!


---

_Renamed from "Not treating @staticmethods as static within an `abc.ABCMeta` (sub)class" to "Not treating `@staticmethod`s as static within an `abc.ABCMeta` (sub)class" by @trevenrawr on 2023-02-07 19:42_

---

_Label `bug` added by @charliermarsh on 2023-02-07 19:52_

---

_Comment by @charliermarsh on 2023-02-07 19:52_

Thanks for filing! I'll take a look.

---

_Closed by @charliermarsh on 2023-02-07 19:57_

---
