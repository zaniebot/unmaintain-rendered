```yaml
number: 7990
title: "Rename use of `--format` in `ruff rule` to `--output-format`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-10-16T19:05:59Z
updated_at: 2023-10-28T03:30:47Z
url: https://github.com/astral-sh/ruff/issues/7990
synced_at: 2026-01-12T15:54:47Z
```

# Rename use of `--format` in `ruff rule` to `--output-format`

---

_@zanieb_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
As with #7354, we should probably rename `ruff rule --format ...` to `ruff rule --output-format ...` for clarity.

The old option should be deprecated then removed later.


---

_Label `cli` added by @zanieb on 2023-10-16 19:05_

---

_Comment by @Skylion007 on 2023-10-16 21:58_

I got bitten by this when upgrading ruff as well.

---

_Comment by @sibocw on 2023-10-17 13:18_

Yes... This broke my Github Action that uses ruff. I guessed something like this could be the reason, but it wasn't obvious to me from the ruff documentation that `--format` is now `--output-format`

---

_Closed by @charliermarsh on 2023-10-28 03:30_

---
