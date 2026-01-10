```yaml
number: 8040
title: Avoid raising S310 on string literals
type: issue
state: closed
author: ZeeD
labels:
  - bug
assignees: []
created_at: 2023-10-18T10:03:08Z
updated_at: 2023-10-18T15:06:15Z
url: https://github.com/astral-sh/ruff/issues/8040
synced_at: 2026-01-10T11:09:50Z
```

# Avoid raising S310 on string literals

---

_Issue opened by @ZeeD on 2023-10-18 10:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

`s310literal.py`

```python
from urllib.request import urlopen

urlopen("http://www.example.com")
```

command:

```sh
$ ruff check --select S310 s310literal.py
s310literal.py:3:1: S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
Found 1 error.
```

`ruff rule S310` suggest

> To mitigate this risk, audit all uses of URL open functions and ensure that
only permitted schemes are used (e.g., allowing `http:` and `https:` and
disallowing `file:` and `ftp:`).

but in this case there is nothing to check...

---

_Label `bug` added by @charliermarsh on 2023-10-18 13:45_

---

_Comment by @charliermarsh on 2023-10-18 13:45_

👍 We can do better here.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-18 14:06_

---

_Closed by @charliermarsh on 2023-10-18 14:36_

---

_Comment by @ZeeD on 2023-10-18 15:06_

thanks!!!

---
