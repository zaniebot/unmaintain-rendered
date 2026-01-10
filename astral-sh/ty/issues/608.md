```yaml
number: 608
title: "documentation for `python-version` should be updated to mention that it defaults to the minimum `project.requires-python` value from `pyproject.toml`"
type: issue
state: closed
author: DetachHead
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-06-08T01:43:22Z
updated_at: 2025-06-09T13:32:45Z
url: https://github.com/astral-sh/ty/issues/608
synced_at: 2026-01-10T02:34:10Z
```

# documentation for `python-version` should be updated to mention that it defaults to the minimum `project.requires-python` value from `pyproject.toml`

---

_Issue opened by @DetachHead on 2025-06-08 01:43_

[the docs for `python-version`](https://github.com/astral-sh/ty/blob/main/docs/reference/configuration.md#python-version) say that it defaults to 3.13, but i noticed that it actually defaults to python 3.9 in my project. i realized this is because i have the following in my `pyproject.toml`:

```toml
[project]
requires-python = ">=3.9,<4.0"
```

i love that ty does this, because i've always found it annoying that i've had to add redundant python version config for other type checkers. however i think the docs should be updated to mention this behavior.

---

_Renamed from "documentation for `python-version` should be updated to mention that it defaults to the minimum `requires-python` value" to "documentation for `python-version` should be updated to mention that it defaults to the minimum `project.requires-python` value from `pyproject.toml`" by @DetachHead on 2025-06-08 01:45_

---

_Label `documentation` added by @MichaReiser on 2025-06-08 05:48_

---

_Label `help wanted` added by @MichaReiser on 2025-06-08 05:48_

---

_Comment by @MichaReiser on 2025-06-08 05:49_

Thanks. This is explained in the `docs/README.md`, but we should update the CLI and configuration documentation with the latest changes we made to the python version inference.

---

_Closed by @AlexWaygood on 2025-06-09 13:32_

---
