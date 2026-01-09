---
number: 4321
title: "[Autofix error] FLY002 fails with `f\"{', '.join([])}\"`"
type: issue
state: closed
author: konstin
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-05-09T18:39:36Z
updated_at: 2023-05-18T14:33:36Z
url: https://github.com/astral-sh/ruff/issues/4321
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] FLY002 fails with `f"{', '.join([])}"`

---

_Issue opened by @konstin on 2023-05-09 18:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In ruff 99a755f936ebd73cdd1bfccbc121c31d4b27751e, the following snippet causes the autofix FLY002 from the flynt rules to introduce a syntax error:

```python
f"{', '.join([])}"
```

```
$ ./ruff --no-cache --select FLY002 --fix code.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `code.py`, the rule codes FLY002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

code.py:1:4: FLY002 Consider `f""` instead of string join
Found 1 error.
```

This was originally discovered in https://github.com/aws/aws-sam-cli/blob/abb6fd069ce79668c7a811758a732970c5be1a66/samcli/commands/init/command.py#L161


---

_Label `bug` added by @konstin on 2023-05-09 18:39_

---

_Label `autofix` added by @konstin on 2023-05-09 18:39_

---

_Label `autofix-error` added by @konstin on 2023-05-09 18:39_

---

_Renamed from "[Autofix error] FLY002 fails with `f"{' '}"`" to "[Autofix error] FLY002 fails with `f"{', '.join([])}"`" by @konstin on 2023-05-09 18:41_

---

_Referenced in [astral-sh/ruff#4487](../../astral-sh/ruff/pulls/4487.md) on 2023-05-18 02:47_

---

_Closed by @charliermarsh on 2023-05-18 14:33_

---
