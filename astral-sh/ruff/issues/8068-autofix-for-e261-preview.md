```yaml
number: 8068
title: Autofix for E261 (preview)
type: issue
state: closed
author: Skylion007
labels:
  - fixes
assignees: []
created_at: 2023-10-19T17:15:34Z
updated_at: 2023-10-22T00:41:07Z
url: https://github.com/astral-sh/ruff/issues/8068
synced_at: 2026-01-10T11:09:50Z
```

# Autofix for E261 (preview)

---

_Issue opened by @Skylion007 on 2023-10-19 17:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
One common issue I have when adding my own noqa rules is forgetting to add the two spaces before the NOQA comments. Link: https://docs.astral.sh/ruff/rules/too-few-spaces-before-inline-comment/ This rule seems like a very easy autofix to add and would probably make a great first issue. Many of the other E rules are also lacking autofixes which would be easy to implement.


---

_Label `autofix` added by @charliermarsh on 2023-10-19 17:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 23:07_

---

_Closed by @charliermarsh on 2023-10-22 00:41_

---
