---
number: 3348
title: Why is a venv needed when doing uv pip install --target ?
type: issue
state: closed
author: gotcha
labels: []
assignees: []
created_at: 2024-05-03T14:32:16Z
updated_at: 2024-05-04T01:39:31Z
url: https://github.com/astral-sh/uv/issues/3348
synced_at: 2026-01-07T13:12:17-06:00
---

# Why is a venv needed when doing uv pip install --target ?

---

_Issue opened by @gotcha on 2024-05-03 14:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Thanks a lot for adding the `--target` option to `uv pip install`.

I am using 0.1.39.

I am surprised that I do need to create a `venv` to be allowed to run `uv pip install --target`.

```
$ bin/uv pip install --target bla zope.component
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.

$ bin/uv venv
Using Python 3.11.6 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ bin/uv pip install --target bla --no-deps zope.component
Resolved 1 package in 5ms
Installed 1 package in 2ms
 + zope-component==6.0
# success !
```

Is there a reason that I am missing ?


---

_Comment by @charliermarsh on 2024-05-03 14:47_

It's consistent with our rules for Python discovery. You can run with `--system` if you want to discover the system Python.

---

_Closed by @charliermarsh on 2024-05-04 01:39_

---

_Referenced in [astral-sh/uv#9356](../../astral-sh/uv/issues/9356.md) on 2024-11-22 20:33_

---
