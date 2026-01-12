```yaml
number: 9214
title: "Using `--frozen` causes `--package` selection to be ignored"
type: issue
state: closed
author: mattfysh
labels:
  - bug
assignees: []
created_at: 2024-11-19T02:51:16Z
updated_at: 2024-11-19T03:18:02Z
url: https://github.com/astral-sh/uv/issues/9214
synced_at: 2026-01-12T15:59:44Z
```

# Using `--frozen` causes `--package` selection to be ignored

---

_@mattfysh_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Reproduction repo here: https://github.com/mattfysh/uvsyncmono

Pass criteria: `uv sync --frozen --package foo` should not install baz or baz's dependencies (dpath)

* **uv version**: uv 0.5.2 (Homebrew 2024-11-14)
* **os**: Darwin 24.0.0 arm64 arm

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-19 03:00_

---

_Label `bug` added by @charliermarsh on 2024-11-19 03:00_

---

_Comment by @charliermarsh on 2024-11-19 03:00_

Thanks! (We confirmed that this only applies to workspaces without a `[project]` table.)

---

_Closed by @charliermarsh on 2024-11-19 03:18_

---

_Closed by @charliermarsh on 2024-11-19 03:18_

---
