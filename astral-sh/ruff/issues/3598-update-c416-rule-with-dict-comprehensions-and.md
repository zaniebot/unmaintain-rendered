---
number: 3598
title: Update C416 rule with dict comprehensions and make autofixable
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - rule
assignees: []
created_at: 2023-03-18T18:03:51Z
updated_at: 2023-03-19T18:37:29Z
url: https://github.com/astral-sh/ruff/issues/3598
synced_at: 2026-01-10T01:22:42Z
---

# Update C416 rule with dict comprehensions and make autofixable

---

_Issue opened by @Skylion007 on 2023-03-18 18:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I recently [updated flake8-comprehension's C416](https://github.com/adamchainz/flake8-comprehensions/pull/490) check to extend to dict comprehensions as well. It should also be auto-fixable just like the other C416 checks. Since the new version is now released on PyPy, it is a good idea to update to it.


---

_Comment by @charliermarsh on 2023-03-18 18:11_

Sweet, thanks for the heads up.

---

_Label `good first issue` added by @charliermarsh on 2023-03-18 18:12_

---

_Label `rule` added by @charliermarsh on 2023-03-18 18:12_

---

_Referenced in [sympy/sympy#24935](../../sympy/sympy/issues/24935.md) on 2023-03-19 01:59_

---

_Comment by @dhruvmanila on 2023-03-19 09:15_

Hi, can I take this?

---

_Referenced in [astral-sh/ruff#3605](../../astral-sh/ruff/pulls/3605.md) on 2023-03-19 09:40_

---

_Assigned to @dhruvmanila by @MichaReiser on 2023-03-19 12:40_

---

_Closed by @charliermarsh on 2023-03-19 18:37_

---
