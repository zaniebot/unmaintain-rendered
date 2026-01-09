---
number: 2501
title: uv fails to install wheel from path with fragment
type: issue
state: closed
author: eth3lbert
labels:
  - bug
assignees: []
created_at: 2024-03-18T01:20:46Z
updated_at: 2024-03-18T17:06:13Z
url: https://github.com/astral-sh/uv/issues/2501
synced_at: 2026-01-07T13:12:17-06:00
---

# uv fails to install wheel from path with fragment

---

_Issue opened by @eth3lbert on 2024-03-18 01:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Currently, `uv` is unable to install a wheel from a path that includes a fragment `(valid in pip)`.
```console
$ uv pip install 'python_dateutil @ /tmp/python_dateutil-2.9.0.post0-py2.py3-none-any.whl#hash=somehash'
error: Distribution not found at: file:///tmp/python_dateutil-2.9.0.post0-py2.py3-none-any.whl%23hash=somehash
```

---

_Label `bug` added by @charliermarsh on 2024-03-18 01:21_

---

_Comment by @charliermarsh on 2024-03-18 01:21_

Thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-18 01:24_

---

_Referenced in [astral-sh/uv#2502](../../astral-sh/uv/pulls/2502.md) on 2024-03-18 01:30_

---

_Closed by @charliermarsh on 2024-03-18 17:06_

---
