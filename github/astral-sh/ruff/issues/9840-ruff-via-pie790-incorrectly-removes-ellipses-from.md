---
number: 9840
title: ruff (via PIE790) incorrectly removes ellipses from Protocols with generics
type: issue
state: closed
author: Julian
labels:
  - bug
assignees: []
created_at: 2024-02-05T18:50:12Z
updated_at: 2024-02-05T19:36:18Z
url: https://github.com/astral-sh/ruff/issues/9840
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff (via PIE790) incorrectly removes ellipses from Protocols with generics

---

_Issue opened by @Julian on 2024-02-05 18:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

On ruff 0.2.0, the following command:

```
⊙  ruff --version && ruff --select PIE --isolated <(printf '                                                                                                                                                                                                                                                        julian@Airm
from typing import Protocol, TypeVar


D = TypeVar("D")


class Foo(Protocol[D]):
    """
    Foo bar.
    """
    ...
')
```

produces:

```
ruff 0.2.0
/dev/fd/11:12:5: PIE790 [*] Unnecessary `...` literal
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

but the ellipsis should not be removed.

This is essentially the same issue as #8756 but it looks like the fix put in there only handled non-generic Protocols.


---

_Comment by @charliermarsh on 2024-02-05 18:50_

Ah thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-05 18:50_

---

_Label `bug` added by @charliermarsh on 2024-02-05 18:50_

---

_Comment by @charliermarsh on 2024-02-05 18:50_

I'll fix this now and it can go out in today's release.

---

_Comment by @Julian on 2024-02-05 18:51_

You sir are fast :) Much appreciation as usual!

---

_Referenced in [astral-sh/ruff#9841](../../astral-sh/ruff/pulls/9841.md) on 2024-02-05 18:52_

---

_Comment by @charliermarsh on 2024-02-05 18:52_

No prob :)

---

_Closed by @charliermarsh on 2024-02-05 19:36_

---
