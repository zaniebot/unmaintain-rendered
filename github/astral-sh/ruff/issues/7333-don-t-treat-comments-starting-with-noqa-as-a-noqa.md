---
number: 7333
title: "Don't treat comments starting with `noqa` as a `noqa` comment if they're clearly not"
type: issue
state: closed
author: jakkdl
labels:
  - suppression
assignees: []
created_at: 2023-09-13T13:01:50Z
updated_at: 2024-01-04T04:00:48Z
url: https://github.com/astral-sh/ruff/issues/7333
synced_at: 2026-01-07T13:12:15-06:00
---

# Don't treat comments starting with `noqa` as a `noqa` comment if they're clearly not

---

_Issue opened by @jakkdl on 2023-09-13 13:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
**example file**
```python
# noqa is used extensively in this file [...]
```
**output**
```
$ ruff --select ALL foo.py
...
warning: Invalid `# noqa` directive on foo.py:1: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
...
```
(using `--isolated` removes the warning)

No settings, version `0.0.289`.

**Real life incident**: https://github.com/python-trio/trio/pull/2795/files/78d67c11d99d64c19f0e5182608b6653ad32924b#r1323337111

**Minimal suggested fix**: Ignore warning if the line is blank, and the comment has trailing characters after `noqa[^:]`.

---

_Referenced in [python-trio/trio#2795](../../python-trio/trio/pulls/2795.md) on 2023-09-13 13:15_

---

_Label `noqa` added by @charliermarsh on 2023-09-13 17:21_

---

_Comment by @charliermarsh on 2024-01-04 04:00_

I think my preference is to retain our current behavior. I know it's annoying in certain cases, but it's possible that the user wrote a line that starts with `# noqa` followed by an explanation and expected it to work as a blanket ignore comment.

---

_Closed by @charliermarsh on 2024-01-04 04:00_

---
