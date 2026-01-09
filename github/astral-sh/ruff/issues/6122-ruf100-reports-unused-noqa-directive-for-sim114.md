---
number: 6122
title: RUF100 reports unused noqa directive for SIM114, but it is not unused
type: issue
state: closed
author: tkukushkin
labels:
  - bug
assignees: []
created_at: 2023-07-27T11:21:00Z
updated_at: 2023-07-27T13:32:11Z
url: https://github.com/astral-sh/ruff/issues/6122
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF100 reports unused noqa directive for SIM114, but it is not unused

---

_Issue opened by @tkukushkin on 2023-07-27 11:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Command:
```
ruff check --isolated --select RUF100,SIM114 foo.py
```

foo.py:
```python
if foo:  # noqa: SIM114
    do_something()
elif bar:
    do_something()
```

Ruff reports:

```
RUF100 [*] Unused `noqa` directive (unused: `SIM114`)
```

If I remove noqa directive, it reports:

```
SIM114 Combine `if` branches using logical `or` operator
```

ruff version: 0.0.280

I think the problem appeared in 0.0.279, 0.0.278 doesn't have this problem.



---

_Comment by @charliermarsh on 2023-07-27 13:32_

I believe this is fixed by #6031.

---

_Closed by @charliermarsh on 2023-07-27 13:32_

---

_Label `bug` added by @charliermarsh on 2023-07-27 13:32_

---
