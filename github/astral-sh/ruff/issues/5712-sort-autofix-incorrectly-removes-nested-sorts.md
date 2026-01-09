---
number: 5712
title: sort autofix incorrectly removes nested sorts
type: issue
state: closed
author: BorisOrca
labels:
  - bug
  - good first issue
  - help wanted
  - accepted
assignees: []
created_at: 2023-07-12T13:49:58Z
updated_at: 2023-07-14T13:43:48Z
url: https://github.com/astral-sh/ruff/issues/5712
synced_at: 2026-01-07T13:12:15-06:00
---

# sort autofix incorrectly removes nested sorts

---

_Issue opened by @BorisOrca on 2023-07-12 13:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Minimal reproduction:

```python
values = [(2, "b"), (2, "a"), (1, "b"), (1, "a")]
print(sorted(sorted(values, key=lambda x: x[1]), key=lambda x: x[0]))
```
prints out: `[(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]`

After running `ruff check reproduce_sort_issue.py`
``` Found 1 error (1 fixed, 0 remaining).```

The "fixed" code is:
```python
values = [(2, "b"), (2, "a"), (1, "b"), (1, "a")]
print(sorted(values, key=lambda x: x[0]))
```

prints out: `[(1, 'b'), (1, 'a'), (2, 'b'), (2, 'a')]`

```
> ruff --version
ruff 0.0.277
```
 


---

_Label `bug` added by @charliermarsh on 2023-07-12 13:52_

---

_Label `accepted` added by @charliermarsh on 2023-07-12 13:52_

---

_Comment by @charliermarsh on 2023-07-12 13:53_

Looks like we need to avoid collapsing sorts with different keys.

---

_Label `good first issue` added by @charliermarsh on 2023-07-12 18:22_

---

_Label `help wanted` added by @charliermarsh on 2023-07-12 18:22_

---

_Referenced in [astral-sh/ruff#5761](../../astral-sh/ruff/pulls/5761.md) on 2023-07-14 12:48_

---

_Closed by @charliermarsh on 2023-07-14 13:43_

---
