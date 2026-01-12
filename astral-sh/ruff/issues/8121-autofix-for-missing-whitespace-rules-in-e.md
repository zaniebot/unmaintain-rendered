```yaml
number: 8121
title: Autofix for missing whitespace rules in E pycodestyle (preview)
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2023-10-22T18:42:41Z
updated_at: 2023-10-23T18:53:13Z
url: https://github.com/astral-sh/ruff/issues/8121
synced_at: 2026-01-12T15:54:47Z
```

# Autofix for missing whitespace rules in E pycodestyle (preview)

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

- [x] E225
- [x] E226
- [x] E227
- [x] E228
- [X] E231
- [x] E252
- [x] E275

All these rules are very similar, already implemented, and have a trivial autofix that should be added. It would also be a good first issue since the autofix is an easy one to write. @charliermarsh you may want to add a `good-first-issue` label.

---

_Renamed from "Add autofix for missing whitespace rules in E prefix (preview)" to "Autofix for missing whitespace rules in E prefix (preview)" by @Skylion007 on 2023-10-22 18:42_

---

_Renamed from "Autofix for missing whitespace rules in E prefix (preview)" to "Autofix for missing whitespace rules in E pycodestyle (preview)" by @Skylion007 on 2023-10-22 18:43_

---

_Label `autofix` added by @dhruvmanila on 2023-10-23 02:15_

---

_Label `good first issue` added by @charliermarsh on 2023-10-23 03:55_

---

_Comment by @reswqa on 2023-10-23 10:51_

Thanks, I'd like to take this to better understand `ruff` code base.

---

_Closed by @charliermarsh on 2023-10-23 18:53_

---

_Comment by @charliermarsh on 2023-10-23 18:53_

Thanks @reswqa! Great work!

---
