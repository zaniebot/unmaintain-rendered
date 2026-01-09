---
number: 13006
title: "add `--show-files` for `ruff format`"
type: issue
state: open
author: Enayaaa
labels:
  - cli
  - wish
assignees: []
created_at: 2024-08-20T10:33:16Z
updated_at: 2024-08-20T11:00:50Z
url: https://github.com/astral-sh/ruff/issues/13006
synced_at: 2026-01-07T13:12:15-06:00
---

# add `--show-files` for `ruff format`

---

_Issue opened by @Enayaaa on 2024-08-20 10:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I am using `ruff check --show-files` to check which files would be checked and since linting and formatting has it's own `exclude` settings and so on in the config it would be useful to have `ruff format --show-files` to check which files would be checked for formatting.

I am using the output to control which files should be checked in a dynamic setting and cannot do the same for formatting check since the `include` and `exclude` setting for linting and formatting might be different.

```shell
user ~ ➜  ruff --version
ruff 0.6.1
```

---

_Label `cli` added by @MichaReiser on 2024-08-20 11:00_

---

_Label `wish` added by @MichaReiser on 2024-08-20 11:00_

---

_Referenced in [astral-sh/ruff#13027](../../astral-sh/ruff/issues/13027.md) on 2024-08-21 09:19_

---
