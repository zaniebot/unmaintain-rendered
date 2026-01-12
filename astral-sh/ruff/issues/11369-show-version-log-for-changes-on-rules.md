```yaml
number: 11369
title: Show version log for changes on rules
type: issue
state: open
author: inoa-jboliveira
labels:
  - documentation
  - needs-design
assignees: []
created_at: 2024-05-11T15:19:03Z
updated_at: 2024-05-11T18:11:46Z
url: https://github.com/astral-sh/ruff/issues/11369
synced_at: 2026-01-12T15:54:51Z
```

# Show version log for changes on rules

---

_@inoa-jboliveira_

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

Context: Discussion https://github.com/astral-sh/ruff/discussions/11367

The idea is that each rule should have a history or log of when it was added as preview, when it became stable and when it had changes in behavior. Also the version if it became deprecated or removed.

Example:

Rule FOO123
v0.1.2 - Added as experimental
v0.2.7 - Behavior changed for classes with letter J
v0.3.0 - Rule became stable
v0.3.2 - Rule deprecated in favor of BAR999
v0.4.0 - Rule removed

This page https://docs.astral.sh/ruff/rules/ could show which minor version the rule became stable if it is stable or the patch version if it is still experimental:

| Code  | Name  | Message  | Availability |  |
| --- | --- | --- | --- | --- | 
| FOO201 | class-comparison-one | Checks when class is compared to function |  0.2 | ✔️ 🛠️ | 
| FOO202 | class-comparison-two | Checks when class is compared to method |  0.3.4  | 🧪 🛠️ | 

In our application we have preview = true because we are moving quite fast with ruff iterations and even experimental ruff rules are usually much better than relying on other linters. Having this would help a lot on us prioritizing rules that been experimental for longer or that have been added on versions that our devs have installed already.




---

_Label `documentation` added by @charliermarsh on 2024-05-11 16:17_

---

_Label `needs-design` added by @zanieb on 2024-05-11 18:11_

---

_Comment by @zanieb on 2024-05-11 18:11_

We'd need to design internal tooling to make this possible.

---
