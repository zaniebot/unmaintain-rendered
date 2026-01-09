---
number: 7960
title: "New rule idea: alphabetic `__all__`"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-10-14T16:09:54Z
updated_at: 2023-10-14T19:52:26Z
url: https://github.com/astral-sh/ruff/issues/7960
synced_at: 2026-01-07T13:12:15-06:00
---

# New rule idea: alphabetic `__all__`

---

_Issue opened by @jamesbraza on 2023-10-14 16:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I am using `ruff==0.0.292`.

I think a nifty rule would be to alphabetize the values in a module's `__all__`.  An alphabetized `__all__` is easier to skim through than an unsorted `__all__`.

---

_Comment by @charliermarsh on 2023-10-14 18:29_

I believe this is similar to #1198!

---

_Closed by @charliermarsh on 2023-10-14 18:29_

---

_Comment by @jamesbraza on 2023-10-14 19:52_

Sorry for the dupe 👌 

---
