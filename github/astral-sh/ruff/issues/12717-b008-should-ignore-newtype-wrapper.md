---
number: 12717
title: B008 should ignore NewType wrapper
type: issue
state: closed
author: mezuzza
labels:
  - type-inference
assignees: []
created_at: 2024-08-06T15:13:09Z
updated_at: 2025-01-29T10:26:18Z
url: https://github.com/astral-sh/ruff/issues/12717
synced_at: 2026-01-07T13:12:15-06:00
---

# B008 should ignore NewType wrapper

---

_Issue opened by @mezuzza on 2024-08-06 15:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

While it is possible to handle this using `extend-immutable-calls`, you'd have to manually add all NewType wrappers into the list if the NewType is using an immutable underlying type. The user experience would be much simpler and nicer to just use the underlying type of the NewType.

Of course, this would require some amount of type level knowledge and cross-file knowledge so I know that it'd be hard for ruff, but it also certainly doesn't fit in a type checker.


---

_Label `type-inference` added by @charliermarsh on 2024-08-06 18:08_

---

_Referenced in [astral-sh/ruff#15765](../../astral-sh/ruff/pulls/15765.md) on 2025-01-27 07:44_

---

_Closed by @dhruvmanila on 2025-01-29 10:26_

---

_Closed by @dhruvmanila on 2025-01-29 10:26_

---
