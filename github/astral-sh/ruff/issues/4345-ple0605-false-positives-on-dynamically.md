---
number: 4345
title: PLE0605 false positives on dynamically constructed lists
type: issue
state: closed
author: Skylion007
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-05-10T14:37:14Z
updated_at: 2023-06-08T17:19:58Z
url: https://github.com/astral-sh/ruff/issues/4345
synced_at: 2026-01-07T13:12:14-06:00
---

# PLE0605 false positives on dynamically constructed lists

---

_Issue opened by @Skylion007 on 2023-05-10 14:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
PLE0605 has false positive when constructing a list from other lists. Specifically https://github.com/pytorch/pytorch/blob/f542b31c9dea1937328d3c3ce03738ce340faf1e/torch/distributed/rpc/__init__.py#L81 gets flagged by ruff as does https://github.com/pytorch/pytorch/blob/f542b31c9dea1937328d3c3ce03738ce340faf1e/torch/multiprocessing/__init__.py#L28
ruff 0.

---

_Referenced in [pytorch/pytorch#101079](../../pytorch/pytorch/pulls/101079.md) on 2023-05-10 14:48_

---

_Label `bug` added by @charliermarsh on 2023-05-10 14:51_

---

_Label `type-inference` added by @charliermarsh on 2023-05-10 14:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-19 12:59_

---

_Referenced in [astral-sh/ruff#4967](../../astral-sh/ruff/pulls/4967.md) on 2023-06-08 17:10_

---

_Closed by @charliermarsh on 2023-06-08 17:19_

---
