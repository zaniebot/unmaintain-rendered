```yaml
number: 12034
title: "Add `np.ComplexWarning`, etc., to NPY201"
type: issue
state: closed
author: njzjz
labels:
  - rule
assignees: []
created_at: 2024-06-25T22:00:16Z
updated_at: 2024-06-27T18:56:58Z
url: https://github.com/astral-sh/ruff/issues/12034
synced_at: 2026-01-12T15:54:51Z
```

# Add `np.ComplexWarning`, etc., to NPY201

---

_@njzjz_

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
Warnings and exceptions present in `numpy.exceptions` (e.g, `~numpy.exceptions.ComplexWarning`,  `~numpy.exceptions.VisibleDeprecationWarning`) are no longer exposed in the main namespace.

- np.ComplexWarning
- np.VisibleDeprecationWarning
- np.RankWarning
- np.AxisError
- np.DTypePromotionError
- np.TooHardError

See: https://github.com/numpy/numpy/pull/25966/

---

_Comment by @charliermarsh on 2024-06-26 20:59_

\cc @mtsokol 

---

_Label `rule` added by @charliermarsh on 2024-06-26 21:00_

---

_Comment by @mtsokol on 2024-06-27 10:38_

It will be fixed in https://github.com/astral-sh/ruff/pull/12065.

---

_Comment by @charliermarsh on 2024-06-27 18:56_

Thanks @mtsokol!

---

_Closed by @charliermarsh on 2024-06-27 18:56_

---
