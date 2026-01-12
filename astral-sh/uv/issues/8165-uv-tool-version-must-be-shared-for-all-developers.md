```yaml
number: 8165
title: uv tool version must be shared for all developers on a project
type: issue
state: closed
author: benjs
labels:
  - question
assignees: []
created_at: 2024-10-14T06:27:00Z
updated_at: 2024-10-14T13:46:05Z
url: https://github.com/astral-sh/uv/issues/8165
synced_at: 2026-01-12T15:59:21Z
```

# uv tool version must be shared for all developers on a project

---

_@benjs_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
We very much like the "uv tool" command, because tools can be installed in a separate venv.

However, when working with a CI and multiple developers, everyone needs to know what the current version of the tool is.
It would be nice if the tool versions can be shared, e.g. via the pyproject.toml.

uv version:
```
uv 0.4.18
```

---

_Comment by @charliermarsh on 2024-10-14 13:46_

I believe we have a design for "project-level tools" in https://github.com/astral-sh/uv/issues/3560. I think that will cover the use-case you're describing -- gonna merge into that issue.

---

_Closed by @charliermarsh on 2024-10-14 13:46_

---

_Label `question` added by @charliermarsh on 2024-10-14 13:46_

---
