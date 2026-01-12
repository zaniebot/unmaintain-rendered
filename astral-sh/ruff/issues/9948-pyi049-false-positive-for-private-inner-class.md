```yaml
number: 9948
title: "PYI049: false positive for private inner class"
type: issue
state: closed
author: arvidfm
labels:
  - rule
assignees: []
created_at: 2024-02-12T13:19:46Z
updated_at: 2024-02-12T17:06:21Z
url: https://github.com/astral-sh/ruff/issues/9948
synced_at: 2026-01-12T15:54:49Z
```

# PYI049: false positive for private inner class

---

_@arvidfm_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The following code results in a PYI049 error even though `_B` is very much used:

```py
from typing import TypedDict


class A:
    class _B(TypedDict):
        pass

    def f(self) -> None:
        print(A._B())


A().f()
```

```console
$ ruff check --select=PYI test.py
test.py:5:11: PYI049 Private TypedDict `_B` is never used
Found 1 error.
$ ruff --version
ruff 0.2.1
```

---

_Label `rule` added by @AlexWaygood on 2024-02-12 13:20_

---

_Comment by @charliermarsh on 2024-02-12 14:47_

We should avoid flagging these when they're defined in a class scope. @AlexWaygood just because I see you on this issue, do you want to fix? Otherwise I can.

---

_Comment by @AlexWaygood on 2024-02-12 14:52_

> @AlexWaygood just because I see you on this issue, do you want to fix? Otherwise I can.

I'm super busy this week at work -- I have a lot of handover stuff to be getting on with :(

If you could take a look, that would be great!

---

_Comment by @charliermarsh on 2024-02-12 14:52_

No prob, on it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-12 14:52_

---

_Closed by @charliermarsh on 2024-02-12 17:06_

---
