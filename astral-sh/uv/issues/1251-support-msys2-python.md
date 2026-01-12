```yaml
number: 1251
title: Support msys2 python
type: issue
state: closed
author: konstin
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-05T16:40:10Z
updated_at: 2024-03-01T14:48:33Z
url: https://github.com/astral-sh/uv/issues/1251
synced_at: 2026-01-12T15:58:26Z
```

# Support msys2 python

---

_@konstin_

See https://github.com/PyO3/maturin/issues/1108. The are multiple places in the code that currently assume that windows uses `Scripts` and not `bin`.

---

_Label `bug` added by @konstin on 2024-02-05 16:40_

---

_Label `windows` added by @konstin on 2024-02-05 16:40_

---

_Comment by @charliermarsh on 2024-03-01 14:48_

We have some handling for this -- _should_ be ok?

---

_Closed by @charliermarsh on 2024-03-01 14:48_

---
