---
number: 1949
title: "[Feature Request] Support the `--isolated` flag for pip"
type: issue
state: closed
author: TalAmuyal
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-24T08:58:22Z
updated_at: 2024-05-22T13:31:08Z
url: https://github.com/astral-sh/uv/issues/1949
synced_at: 2026-01-07T13:12:16-06:00
---

# [Feature Request] Support the `--isolated` flag for pip

---

_Issue opened by @TalAmuyal on 2024-02-24 08:58_

First, thanks for investing in Python's ecosystem!

**Context:**

I'm trying to incorporate `uv` into the flow of our devs at my company.

From the output of `python -m pip --help`:

```
  --isolated           Run pip in an isolated mode, ignoring environment variables and user configuration.
```

**The request:**

I would imagine that `uv pip --isolated ...` will only impact the `pip` "environment variables and user configuration" and won't change behavior for uv's specifics.

For example, it will cause `uv pip install` to ignore `PIP_INDEX_URL`, but not `VIRTUAL_ENV`.

I hope it makes sense.

Thanks!

---

_Label `enhancement` added by @zanieb on 2024-02-24 16:41_

---

_Label `compatibility` added by @zanieb on 2024-02-24 16:41_

---

_Comment by @charliermarsh on 2024-05-22 13:31_

We do have `--isolated` which ignores configuration files but not environment variables. I think that's ok for now?

---

_Closed by @charliermarsh on 2024-05-22 13:31_

---
