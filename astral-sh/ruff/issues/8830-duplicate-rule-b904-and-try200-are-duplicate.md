---
number: 8830
title: "Duplicate rule `B904` and `TRY200` are duplicate"
type: issue
state: closed
author: Skylion007
labels:
  - rule
assignees: []
created_at: 2023-11-23T19:20:42Z
updated_at: 2024-02-01T19:35:06Z
url: https://github.com/astral-sh/ruff/issues/8830
synced_at: 2026-01-10T01:22:48Z
---

# Duplicate rule `B904` and `TRY200` are duplicate

---

_Issue opened by @Skylion007 on 2023-11-23 19:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Rules `B904` and `TRY200` appear to be looking for the exact same construct (not using from within an exception block), but are separate rules. These should be deduplicated to a common implementation with an alias. We should also probably try to find duplicate rules with an automated mechanism in the future.


---

_Comment by @charliermarsh on 2023-11-24 15:22_

Yeah these do look like exact duplicates. We should remove `TRY200`. Thanks for noticing this.

---

_Label `rule` added by @charliermarsh on 2023-11-24 15:22_

---

_Referenced in [astral-sh/ruff#7993](../../astral-sh/ruff/issues/7993.md) on 2023-11-24 15:22_

---

_Comment by @Skylion007 on 2023-11-24 18:24_

Ah apparently, this itself is a duplicate issue: See https://github.com/astral-sh/ruff/issues/8736

---

_Referenced in [astral-sh/ruff#9461](../../astral-sh/ruff/issues/9461.md) on 2024-01-11 01:18_

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Assigned to @zanieb by @zanieb on 2024-01-30 01:21_

---

_Referenced in [astral-sh/ruff#9680](../../astral-sh/ruff/pulls/9680.md) on 2024-01-30 19:18_

---

_Closed by @zanieb on 2024-02-01 19:35_

---
