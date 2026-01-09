---
number: 11582
title: "Should `[lint.flake8-implicit-str-concat] allow-multiline = false` implicitly disable `ISC003` ?"
type: issue
state: closed
author: Avasam
labels:
  - configuration
  - help wanted
  - accepted
assignees: []
created_at: 2024-05-28T13:08:01Z
updated_at: 2024-07-26T14:36:36Z
url: https://github.com/astral-sh/ruff/issues/11582
synced_at: 2026-01-07T13:12:15-06:00
---

# Should `[lint.flake8-implicit-str-concat] allow-multiline = false` implicitly disable `ISC003` ?

---

_Issue opened by @Avasam on 2024-05-28 13:08_

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


With `allow-multiline = true`, `ISC002` doesn't raise. Should this be reflected on `ISC003` as well? The doc mentions that
https://docs.astral.sh/ruff/settings/#lint_flake8-implicit-str-concat_allow-multiline
> Note that setting allow-multiline = false should typically be coupled with disabling explicit-string-concatenation (ISC003). Otherwise, both explicit and implicit multiline string concatenations will be seen as violations.

I feel like this burden could be lifted from the user, and allowing `--select=ISC` rather than this awkward juggling of `--select=ISC --ignore=ISC003` or `--select=ISC001,ISC002`

---

_Label `configuration` added by @AlexWaygood on 2024-05-28 13:45_

---

_Comment by @charliermarsh on 2024-05-28 18:40_

Seems reasonable to me.

---

_Label `help wanted` added by @charliermarsh on 2024-05-28 21:01_

---

_Label `accepted` added by @charliermarsh on 2024-05-28 21:01_

---

_Referenced in [pypa/setuptools#4411](../../pypa/setuptools/pulls/4411.md) on 2024-06-05 16:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 14:03_

---

_Referenced in [astral-sh/ruff#12532](../../astral-sh/ruff/pulls/12532.md) on 2024-07-26 14:27_

---

_Closed by @charliermarsh on 2024-07-26 14:36_

---

_Closed by @charliermarsh on 2024-07-26 14:36_

---
