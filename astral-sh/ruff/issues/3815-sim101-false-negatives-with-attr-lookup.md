---
number: 3815
title: SIM101 false negatives with attr lookup
type: issue
state: closed
author: Skylion007
labels:
  - rule
assignees: []
created_at: 2023-03-30T14:47:31Z
updated_at: 2023-03-30T16:19:55Z
url: https://github.com/astral-sh/ruff/issues/3815
synced_at: 2026-01-10T01:22:42Z
---

# SIM101 false negatives with attr lookup

---

_Issue opened by @Skylion007 on 2023-03-30 14:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I ran ruff over a large codebase and noticed some differing behavior for SIM101. `assert isinstance(n.args[1], Node) or isinstance(n.args[1], int)` is not detected by SIM101 to merge the isinstance calls. In fact, it looks like any isinstance call which calls an underlying attribute lookup or `__getitem__` function seems to not trigger the ruff SIM101 rule. Is this difference between the two intentional? Or is it just a bug that was overlooked when the rule was implemented?

---

_Comment by @charliermarsh on 2023-03-30 15:01_

It looks like the rule only looks for `isinstance(name, ...)` and not arbitrary expressions. We could support that as long as the expression doesn't contain an effect.

---

_Label `rule` added by @charliermarsh on 2023-03-30 15:01_

---

_Comment by @charliermarsh on 2023-03-30 15:02_

I think this is probably consistent with SIM101 as-is so not labeling a "bug" although it would be good to fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-30 15:51_

---

_Referenced in [astral-sh/ruff#3817](../../astral-sh/ruff/pulls/3817.md) on 2023-03-30 16:05_

---

_Closed by @charliermarsh on 2023-03-30 16:19_

---
