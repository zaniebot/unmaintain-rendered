```yaml
number: 11328
title: "Human-friendly `noqa` directives"
type: issue
state: closed
author: puddly
labels: []
assignees: []
created_at: 2024-05-07T23:45:26Z
updated_at: 2024-05-08T00:04:13Z
url: https://github.com/astral-sh/ruff/issues/11328
synced_at: 2026-01-12T15:54:51Z
```

# Human-friendly `noqa` directives

---

_@puddly_

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

Currently, `noqa` directives need to reference the exact rule prefix and code (e.g. `SLF001`):

```python
device._switch.connect()  # noqa: SLF001
```

It would be nice to reference the rule short name (as in the [documentation](https://docs.astral.sh/ruff/rules/private-member-access/)), as I personally have trouble keeping track of numerical rules:

```python
device._switch.connect()  # noqa: private-member-access
```

This is more of a feature request than a real issue so let me know if this is by design. If not, would you accept a PR implementing this?

---

_Comment by @charliermarsh on 2024-05-08 00:01_

Thanks! We do intend to make this change and there's some discussion on it here: https://github.com/astral-sh/ruff/issues/1773. It hasn't been prioritized yet but there's a very high likelihood that we migrate to human-readable rule names.

---

_Closed by @charliermarsh on 2024-05-08 00:01_

---

_Comment by @puddly on 2024-05-08 00:02_

Oops, sorry for the duplicate!

---

_Comment by @charliermarsh on 2024-05-08 00:04_

All good, only takes me a sec, I know the issue tracker very well :)

---
