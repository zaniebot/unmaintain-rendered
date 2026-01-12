```yaml
number: 13830
title: "Feature request: update deprecated ruff settings automatically"
type: issue
state: open
author: njzjz
labels:
  - configuration
  - cli
assignees: []
created_at: 2024-10-20T09:41:59Z
updated_at: 2024-10-21T08:45:07Z
url: https://github.com/astral-sh/ruff/issues/13830
synced_at: 2026-01-12T15:54:53Z
```

# Feature request: update deprecated ruff settings automatically

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

For the following warning:

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'ignore-init-module-imports' -> 'lint.ignore-init-module-imports'
  - 'select' -> 'lint.select'
  - 'pydocstyle' -> 'lint.pydocstyle'
```

I hope there is a rule or a command to update these deprecation settings automatically. I have many projects so I don't want to update them one by one.

---

_Label `configuration` added by @MichaReiser on 2024-10-21 08:43_

---

_Label `cli` added by @MichaReiser on 2024-10-21 08:43_

---

_Comment by @MichaReiser on 2024-10-21 08:45_

No, there's currently no rule or command to automatically update the configuration. 

But we definitely want this functionality and I'm surprised there isn't an existing issue.

---
