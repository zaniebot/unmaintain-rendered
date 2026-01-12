```yaml
number: 8312
title: Autofix whitespace before or after keyword E prefix (preview)
type: issue
state: closed
author: Skylion007
labels:
  - fixes
assignees: []
created_at: 2023-10-28T18:11:09Z
updated_at: 2023-11-12T00:23:04Z
url: https://github.com/astral-sh/ruff/issues/8312
synced_at: 2026-01-12T15:54:48Z
```

# Autofix whitespace before or after keyword E prefix (preview)

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
Implementing autofix for `E271` and `E272`
`E241`, `E251` is a similar rule that should be fixed

---

_Label `autofix` added by @charliermarsh on 2023-10-28 22:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-11 23:33_

---

_Closed by @charliermarsh on 2023-11-11 23:41_

---

_Comment by @Skylion007 on 2023-11-11 23:56_

Did this also fix E241, E251 is a similar rule that should be fixed? Or should I open another issue for those autofixes?

---

_Comment by @charliermarsh on 2023-11-12 00:23_

Those were fixed in #8623.

---
