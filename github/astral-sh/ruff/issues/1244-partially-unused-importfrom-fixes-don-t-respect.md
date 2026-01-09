---
number: 1244
title: "Partially unused `ImportFrom` fixes don't respect `# noqa`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-12-15T00:31:15Z
updated_at: 2022-12-16T04:13:59Z
url: https://github.com/astral-sh/ruff/issues/1244
synced_at: 2026-01-07T13:12:14-06:00
---

# Partially unused `ImportFrom` fixes don't respect `# noqa`

---

_Issue opened by @charliermarsh on 2022-12-15 00:31_

It just occurred to me that this code:

```py
from module import (
    A,  # noqa: F401
    B,
)
```

Will do the right thing when reporting errors:

```
∴ cargo run foo.py --select F401 -n
    Finished dev [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff foo.py --select F401 -n`
Found 1 error(s).
foo.py:3:2: F401 `module.B` imported but unused
1 potentially fixable with the --fix option.
```

But if you run with `--fix`, it'll remove both imports.


---

_Label `bug` added by @charliermarsh on 2022-12-15 00:31_

---

_Comment by @charliermarsh on 2022-12-15 00:31_

To fix this, we have to move the `# noqa` directive checking into `Checker`.


---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-15 01:22_

---

_Comment by @charliermarsh on 2022-12-15 04:58_

(Definitely need to fix this.)

---

_Referenced in [astral-sh/ruff#1259](../../astral-sh/ruff/pulls/1259.md) on 2022-12-16 04:11_

---

_Closed by @charliermarsh on 2022-12-16 04:13_

---
