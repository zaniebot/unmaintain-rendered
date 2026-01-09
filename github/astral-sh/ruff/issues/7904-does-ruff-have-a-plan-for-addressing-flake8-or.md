---
number: 7904
title: Does ruff have a plan for addressing flake8 or pycodestyle new changes?
type: issue
state: closed
author: mythnc
labels: []
assignees: []
created_at: 2023-10-11T00:07:32Z
updated_at: 2023-10-20T23:27:13Z
url: https://github.com/astral-sh/ruff/issues/7904
synced_at: 2026-01-07T13:12:15-06:00
---

# Does ruff have a plan for addressing flake8 or pycodestyle new changes?

---

_Issue opened by @mythnc on 2023-10-11 00:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

According to ruff documents, [it is stated](https://docs.astral.sh/ruff/faq/#how-does-ruff-compare-to-flake8) that

> Under those conditions, Ruff implements every rule in Flake8. In practice, that means Ruff implements all of the F rules (which originate from Pyflakes), along with a subset of the E and W rules (which originate from pycodestyle).

I'm curious if ruff will continue to follow and implement changes every time flake8 or pycodestyle updates itself?

For instance, pycodestyle recently [made changes](https://github.com/PyCQA/pycodestyle/blob/21abd9b6dcbfa38635bc85a2c2327ec11ad91ffc/CHANGES.txt#L12-L13) in how it handles E721.

> * E721: adjust handling of type comparison.  Allowed forms are now
  ``isinstance(x, t)`` or ``type(x) is t``.  PR `#1086`, `#1167`.

I would like to know if ruff plan to adopt these changes.

Thank you.

---

_Comment by @charliermarsh on 2023-10-11 00:17_

We generally do implement changes that are made upstream, though they sometimes need to be brought to our attention so it's always welcome to file an issue if we've missed something. I'll take a look at the E721 changes, thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-11 00:20_

---

_Comment by @charliermarsh on 2023-10-11 00:20_

I'll update E721 now and use this issue for it.

---

_Referenced in [astral-sh/ruff#7905](../../astral-sh/ruff/pulls/7905.md) on 2023-10-11 00:33_

---

_Closed by @charliermarsh on 2023-10-20 23:27_

---
