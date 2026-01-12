```yaml
number: 9062
title: "Can ruff support `flake8-private-name-import`"
type: issue
state: closed
author: Tomperez98
labels:
  - plugin
assignees: []
created_at: 2023-12-08T23:36:20Z
updated_at: 2023-12-09T16:03:15Z
url: https://github.com/astral-sh/ruff/issues/9062
synced_at: 2026-01-12T15:54:48Z
```

# Can ruff support `flake8-private-name-import`

---

_@Tomperez98_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

* ruff 0.1.2

[flake8-private-name-import](https://pypi.org/project/flake8-private-name-import/) prevents importing private modules. I'd say that ideally only the `__init__.py` script on the same level should be able to access those private modules, but it could be easily solved by adding in the `per-file-ignore` configuration

---

_Comment by @Tomperez98 on 2023-12-08 23:36_

https://github.com/astral-sh/ruff/discussions/9061


---

_Label `plugin` added by @charliermarsh on 2023-12-09 03:19_

---

_Comment by @tjkuson on 2023-12-09 10:14_

I think a rule similar to this is being tracked by the Pylint issue (#970) under `import-private-name` (`C2701`).

---

_Comment by @charliermarsh on 2023-12-09 16:03_

Yeah let's consolidate in favor of the Pylint tracking issue. Thanks for filing :)

---

_Closed by @charliermarsh on 2023-12-09 16:03_

---
