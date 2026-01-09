---
number: 4088
title: "`RUF009` not covered by published documentation"
type: issue
state: closed
author: jakob-keller
labels: []
assignees: []
created_at: 2023-04-25T07:05:32Z
updated_at: 2023-04-25T17:30:39Z
url: https://github.com/astral-sh/ruff/issues/4088
synced_at: 2026-01-07T13:12:14-06:00
---

# `RUF009` not covered by published documentation

---

_Issue opened by @jakob-keller on 2023-04-25 07:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I am not sure which release introduced `RUF009`, but since `ruff==0.0.262` it triggers additional detections in my codebase. I believe the rule is currently undocumented, i.e. is not listed on https://beta.ruff.rs/docs/rules/#ruff-specific-rules-ruf, which makes it hard to determine whether the detections are false positives or not.

---

_Comment by @Czaki on 2023-04-25 08:12_

I build docs locally and there is a description of rules in such docs. So it looks like a bug in the docs deployment process. Same like in #4089 

---

_Renamed from "`RUF009` not covered by documentation" to "`RUF009` not covered by published documentation" by @jakob-keller on 2023-04-25 08:21_

---

_Comment by @charliermarsh on 2023-04-25 15:01_

Looks like the docs failed to build: https://github.com/charliermarsh/ruff/actions/runs/4794125532. Will fix today.

---

_Comment by @jakob-keller on 2023-04-25 15:06_

> Looks like the docs failed to build: https://github.com/charliermarsh/ruff/actions/runs/4794125532. Will fix today.

Fantastic! If you ever feel like getting sponsored for you work, let me know.

---

_Referenced in [astral-sh/ruff#4097](../../astral-sh/ruff/pulls/4097.md) on 2023-04-25 15:58_

---

_Closed by @charliermarsh on 2023-04-25 17:30_

---
