```yaml
number: 8425
title: "Feature request: numpydoc rules"
type: issue
state: open
author: njzjz
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-11-01T22:26:23Z
updated_at: 2025-09-02T06:59:57Z
url: https://github.com/astral-sh/ruff/issues/8425
synced_at: 2026-01-12T15:54:48Z
```

# Feature request: numpydoc rules

---

_@njzjz_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

See https://numpydoc.readthedocs.io/en/latest/validation.html.

It would be nice if ruff implemented some of numpydoc rules that are not supported by pydocstyle, such as `PR10`

```
    "PR10": 'Parameter "{param_name}" requires a space before the colon '
    "separating the parameter name and type",
```

---

_Renamed from "Frature request: numpydoc rules" to "Feature request: numpydoc rules" by @njzjz on 2023-11-02 01:48_

---

_Label `plugin` added by @charliermarsh on 2023-11-02 02:12_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-02 02:12_

---

_Comment by @timj on 2023-12-06 21:34_

ruff supporting numpydoc rules would be incredibly helpful.

---

_Comment by @augustebaum on 2024-10-22 12:41_

+1

---

_Comment by @Tatsh on 2025-03-19 19:40_

Yes I especially like the GL07 rule that has no equivalent in the DOC rule set.

---

_Comment by @alexanderpils on 2025-09-02 06:59_

+1

---
